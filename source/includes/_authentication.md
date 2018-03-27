# Authentication

> To check a JWT for validity, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl -X POST "https://apigw-dev.trendeo.com/jwt-test" \
  -H "Authorization: Bearer {insert_jwt_here}" \
  -H "Content-type: application/json"
```

Authentication is based on JSON Web Tokens, which need to be provided in the Authorization header. The API-Gateway also allows the JWT to be submitted in the URL via a GET-parameter called "jwt" (this might be especially relevant for secured GET endpoints).

`Authorization: Bearer {your jwt goes here!}`

Please refer to the login- and refresh-endpoints described below to understand how to obtain and refresh a valid JWT from the backend. You can check your JWT by sending a POST request to the secured endpoint **/jwt-test**

`POST https://apigw-dev.trendeo.com/jwt-test`

A successful call will give you a warm welcome message, however an invalid / missing token will give you a cruel 4xx code...

# Login

A few words on different authentication methods: There is a certain complexity in offering different authentication methods for the same user base. As example, a user might sign up via Facebook, however then attempting a password reset and logging in via the regular email/password login. Following logic is applied to manage this:
A user can sign up / log in via Facebook any time. If there has not been a user so far, it will be created based on the particular Facebook profile. If a user from another sign-up source (i.e. email/password) already exists, this user is simply logged in.
A newly created facebook user will receive a random password.
It might happen that we cannot read the email of the facebook user (e.g. because the user uses phone number login, has not confirmed his/her email, or has selected to his the email address). In that case, the API generates a random email address with domain "no_facebook_email.trendeo.com". The client should then ask the user for his/her real email address and update it (TODO: THAT ENDPOINT DOES NOT EXIST YET!).
A facebook user might request a password reset, after which (s)he will also be able to log in via regular email/password login.

## Impersonation
There is a certain complexity due to the fact that we allow the users to browse around without authentication, and even create addresses, orders, and so on. Of course, we would like the users to keep their guest user actions even when they log in or sign up. For example: When a user already created an address and added items to cart or wishlist, but only then signs up or logs in, we would like the "real user" to have the address and items in cart / wishlist too. Hence, an "impersonation" process needs to happen, during which those objects are transferred to the "real user".
In current implementation, the following aspects are transferred:

- Addresses
- Cart
- Wishlist entries
- Previous orders

Impersonation can happen during the following processes:

- Login (email/password as well as facebook)
- Sign-up (email/password as well as facebook)

You will see below that those endpoints accept a parameter usually called "guestUserJWT", which needs to contain any previous JWT of the previous guest user.

## Getting a JWT with email/password

> To get a jwt and refresh token with a email/password combo, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/login" \
  -H "Content-type: application/json" \
  --data-raw "{\"username\":\"staging-test-user@trendeo.com\",\"password\":\"staging-test\"}"
```

> The above command returns JSON structured like this:

```json
{
  "jwt": "abc...",
  "refreshToken" : "abc...",
  "username": "user@domain.com"
}
```

The first authentication method is simply providing email and password to the **/authentication/login** endpoint. In response, you will either get an error message (pls see description below), or a pair of JWT and refresh token.

### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/login`

### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
username | yes | Email of the user to log in.
password | yes | Password in plain.
guestUserJWT | no | Any JWT of the previous guest user. In case this parameter exists, impersonation is done from this guest user to the newly logged in user in case of success. The guest user JWT is not checked for correct issuer or expiration. Please check the "Impersonation" section above for more information on this topic.

### Response

A successful request gets a new JWT and refreshToken in return. This means, that a potentially existing refreshToken is not valid any more. The JWT can be used for all access-restricted API endpoints for a small amount of time (several hours), while the refreshToken can be used for an extended period of time (multiple weeks) to obtain a new JWT. Keep the refreshToken very safe and secure, and don't use it anywhere else than for getting a new JWT (API endpoint for that is described further below).

Parameter | Description
--------- | -------
jwt | Your fresh and new JWT for future requests.
refreshToken | Token for getting a new JWT once its expired.
username | Username of logged-in user

## Getting a JWT via Facebook login

The Facebook login / sign-up is a 2-step OAuth process. The particular client first needs to redirect the user to Facebook for confirming the access and acquiring a Facebook access token. This redirect URL is provided by the API via a dedicated endpoint. Afterwards, assuming success, the user is redirected back to our client application with the access token in the URL. This access token has to be provided to another endpoint of the API to finalise the login / sign-up.
This API is primarily supposed to be used by Web clients. Mobile apps are supposed to use the native Facebook SDKs and just use the second endpoint as soon as a proper access token is obtained.

### Getting the redirect URL

> To get the facebook authentication URL, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/social/facebook/token-url" \
  -H "Content-type: application/json" \
  --data-raw "{\"redirectUrl\":\"https://trendeo.com/login\"}"
```

> The above command returns JSON structured like this:

```json
{
  "facebookAuthUrl": "https://oauth.facebook.com/..."
}
```

Please redirect the user to the given URL.

#### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/social/facebook/token-url`

#### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
redirectUrl | yes | The URL to which you want the user to be redirected back to. The user will be redirected back in both the success and the error case; however with different GET parameters. In successful case, the redirect back to our client should contain the access_token parameter which then needs to be sent to the backend API in the next request.

#### Response

The API responds with the facebook redirect URL to which you need to send the user. The URL contains the necessary permissions we would like to get granted (currently just email, public_profile and user_birthday).

Parameter | Description
--------- | -------
facebookAuthUrl | The facebook authentication URL to which you should redirect the user.

### Creating a user / logging in a user with a facebook access token

> To create a user / log a user in with a valid Facebook access token, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/social/facebook" \
  -H "Content-type: application/json" \
  --data-raw "{\"market\":\"DE\",\"accessToken\":\"Af33kd...\"}"
```

> The above command returns JSON structured like this:

```json
{
  "jwt": "abc...",
  "refreshToken" : "abc...",
  "username": "user@trendeo.com"
}
```

#### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/social/facebook`

#### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
market | yes | The 2-symbol market code from which the user is logging in / signing up.
accessToken | yes | The Facebook access token obtained from the redirect. Will be used to access the user profile and get all required details to create a user in our database, if one does not exist yet.
guestUserJWT | no | Any JWT of the previous guest user. In case this parameter exists, impersonation is done from this guest user to the newly logged in user in case of success. The guest user JWT is not checked for correct issuer or expiration. Please check the "Impersonation" section above for more information on this topic.

#### Response

The API responds with the same structure you also see in the regular login (a jwt and a refresh token).
Important: As described in the introduction chapter, we might not be able to retrieve the email address of the user. In that case, an auto-generated email address "{facebook-user-id}@no_facebook_email.trendeo.com" is created. The user should then be asked for her/his real email address and then update the information with another backend call (see change-email endpoint below). You can recognize this by analysing the "username" field of the response object.

Parameter | Description
--------- | -------
jwt | Your fresh and new JWT for future requests.
refreshToken | Token for getting a new JWT once its expired.
username | Username of the logged-in user

### Changing the email address

> To change the email address of a facebook user for whom we did not get the email address, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/social/facebook/change-email" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..." \
  --data-raw "{\"username\":\"testuser@trendeo.com\"}"
```

> The above command returns just a 200 response without body:

#### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/social/facebook/change-email`

#### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
username | yes | The new username / email you would like to store to the user.

#### Response

The API responds with a 200 response without body.
You will get a 400 response in case the logged-in user actually does not have an auto-generated username from us.
Important: This endpoint does not do "impersonation" as described in the initial sections, because this change should happen right after sign-up and there should not be too much activity of that user before.

## Getting a JWT for a guest user

> To get a jwt for a new guest user, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/guestuser" \
  -H "Content-type: application/json" \
  --data-raw "{\"market\":\"DE\"}"
```

> The above command returns JSON structured like this:

```json
{
  "username": "GUEST_Wk3Dqlk...",
  "jwt" : "abc..."
}
```

The guest-user authentication does not use a refreshToken, because no database object is created for guest users.

### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/guestuser`

### Query Parameters

The request needs to contain the previous JWT that was used for the particular user.

Parameter | Required? | Description
--------- | ------- | -----------
market | yes | The 2-symbol market code from which the guest user is coming.

### Response

This API endpoint creates a random guest username as well as a valid JWT for further API communication. The JWT lifetime differs for guest users compared to regular users. Usually, we are using short-lived JWTs together with long-lived refresh tokens. However, guest users exclusively use long-lived JWTs, as a guest user is not stored anywhere. Since a guest user should not be able to do anything critically sensitive (e.g. store credit card details), this slightly less secure mechanism is acceptable for us.

Parameter | Description
--------- | -------
username | A randomly created guest username. It will be used in case anything needs to be stored and associated to that particular user (e.g. cart, wishlist, etc.).
jwt | A long-lived JWT for further API communication.

## Refresh the JWT via your refresh token

> To get a fresh jwt for a user with a valid refreshToken, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/refresh" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..." \
  --data-raw "{\"username\":\"staging-test-user@trendeo.com\",\"refreshToken\":\"abc...\"}"
```

> The above command returns JSON structured like this:

```json
{
  "jwt": "abc...",
  "refreshToken" : "abc...",
  "username": "user@domain.com"
}
```

The used refreshToken is invalid after this request.

### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/refresh`

### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
username | yes | Email of the user whose jwt should be refreshed.
refreshToken | yes | Valid refresh-token of the particular user.

### Response

This API endpoints responds with a fresh JWT for further API communication, and a new refreshToken for that particular user. The previous refresh-token is invalidated and cannot be used any more.
IMPORTANT: This mechanism applies only to registered users who have been signed up with an email/password combo. Facebook users and guest users cannot get a refresh-token.

Parameter | Description
--------- | -------
jwt | A new short-lived JWT for further API communication.
refreshToken | A new, long-lived refresh-token to renew the jwt when it expired.
username | Username of the logged-in user

## Password reset

In caes a user forgot her/his password, a 2-step process has to be performed to reset it. The first step is to simply provide the email address to trigger an email containing a time-restricted link for entering a new password. The second step is when the user clicks on that link, comes to the reset-password-page, and enters a new password.

### Triggering the email

> To trigger a reset-password email for a particular user, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/forgot-password" \
  -H "Content-type: application/json" \
  --data-raw "{\"username\":\"staging-test-user@trendeo.com\",\"market\":\"DE\"}"
```

The above command responds just with a 200 status code without body.

#### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/forgot-password`

#### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
username | yes | Email of the user who wants her/his password reset.
market | yes | Market from which the request is sent. This determines the email language.

#### Response

This API endpoints responds simply with a 200 status code without any body.
The given user will receive an email with a link to the form for resetting the password. The URL contains a time-restricted random string which guarantees that the ability to reset the password for the given account expires after 7 days. Furthermore, it contains the base64-encoded username.
The URL is structured as follows: $BASE_URL/$RANDOM_TOKEN/$BASE64_ENCODED_USERNAME

### Reset the users password

> To change the password of a user, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/change-password" \
  -H "Content-type: application/json" \
  --data-raw "{\"password\":\"test-password\",\"resetPasswordToken\":\"Af33kd...\",\"usernameBase64\":\"AE302...\"}"
```

This API endpoint response simply with a 200 status code without body.

#### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/change-password`

#### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
password | yes | The new password for the user, in plain.
resetPasswordToken | yes | The random string in the reset-password-URL
usernameBase64 | yes | The base64-encoded, lowercased email of the user. Also part of the reset-password-URL (see above)

#### Response

This API endpoint response simply with a 200 status code without body.

### Reset the password while logged in

> To change the password of a user from personal area, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/change-password-logged-in" \
  -H "Authorization: Bearer abcd..." \
  -H "Content-type: application/json" \
  --data-raw "{\"currentPassword\":\"current\",\"newPassword\":\"new\"}"
```

This endpoint should be used for when the user would like to change her/his password while still being logged in. The client should then ask for current password as well as new password and provide it to the backend API for verification and update.
This API endpoint response simply with a 200 status code without body.

#### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/change-password-logged-in`

#### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
currentPassword | yes | The current user password
newPassword | yes | The new password to be set for the logged-in user

#### Response

This API endpoint response simply with a 200 status code without body.

# Creating a new user

> To create a new user, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/authentication/user" \
  -H "Content-type: application/json" \
  --data-raw "{\"username\":\"staging-test-user@trendeo.com\",\"market\":\"DE\",\"firstName\":\"API\",\"lastName\":\"LOVER\",\"password\":\"test-password\",\"newsletterAllowed\":true,\"gender\":\"MALE\",\"birthday\":\"2000-02-01\"}"
```

> The above command returns a valid login, i.e. the new user is logged in immediately:

```json
{
  "jwt": "abc...",
  "refreshToken" : "abc...",
  "username": "user@domain.com"
}
```

## HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/user`

## Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
username | yes | Email of the user who signs up.
market | yes | 2-symbol market identifier from which the user signs up.
firstName | yes | First name
lastName | yes | Last name
password | yes | Plain password
newsletterAllowed | no | Boolean value (true or false) whether user allows to be signed up for our newsletters. Default is false.
gender | no | "MALE" or "FEMALE". Empty by default
birthday | no | Valid date in the format yyyy-MM-dd. Empty by default.
guestUserJWT | no | Any JWT of the previous guest user. In case this parameter exists, impersonation is done from this guest user to the newly logged in user in case of success. The guest user JWT is not checked for correct issuer or expiration. Please check the "Impersonation" section above for more information on this topic.

## Response

This API endpoint creates the user and logs her/him in immediately. Hence, the response contains the short-lived JWT as well as the long-lived refresh-token.
If newsletterAllowed is set to true, the user is subscribed to the mailchimp list of the given market (synchronously).
In case a guest user JWT is given, impersonation happens to the newly created user in case of success. Please refer to the "Impersonation" section above for more information about this topic.

Parameter | Description
--------- | -------
jwt | A new short-lived JWT for further API communication.
refreshToken | A long-lived refresh-token to renew the jwt when it expired.
username | Username of new user

# Get user profile

> To get the profile information of a user, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/authentication/user" \
  -H "Authorization: Bearer abcd..."
```

> The above command returns the basic user data for the profile:

```json
{
    "firstName": "API",
    "lastName": "LOVER",
    "email": "api.lover@trendeo.com",
    "birthday": "2000-01-01",
    "gender": "MALE",
    "phone": "+7123123123"
}
```

## HTTP Request

`GET https://apigw-dev.trendeo.com/authentication/user`

## Response

This API endpoint updates the given field of the logged-in user according to the given JWT. It returns the basic user profile:

Parameter | Description
--------- | -------
firstName | First name
lastName | Last name
gender | "MALE" or "FEMALE"
birthday | Valid date in the format yyyy-MM-dd
phone | Phone number

# Updating the user profile

> To update the details of a user, use this code:

```shell
curl -X PATCH "https://apigw-dev.trendeo.com/authentication/user" \
  -H "Authorization: Bearer abcd..." \
  -H "Content-type: application/json" \
  --data-raw "{\"firstName\":\"API\",\"lastName\":\"LOVER\",\"gender\":\"MALE\",\"birthday\":\"2000-01-01\",\"phone\":\"+7123123123\"}"
```

> The above command returns the basic user data for the profile:

```json
{
    "firstName": "API",
    "lastName": "LOVER",
    "email": "api.lover@trendeo.com",
    "birthday": "2000-01-01",
    "gender": "MALE",
    "phone": "+7123123123"
}
```

## HTTP Request

`PATCH https://apigw-dev.trendeo.com/authentication/user`

## Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
firstName | no | First name
lastName | no | Last name
gender | no | "MALE" or "FEMALE"
birthday | no | Valid date in the format yyyy-MM-dd
phone | no | phone number; any text is allowed, no validation

This endpoint is access-restricted of course, so a valid JWT must be provided.
Empty fields will not be updated, i.e. they will not be cleared in case a value is already set.

## Response

This API endpoint updates the given field of the logged-in user according to the given JWT. It returns the basic user profile:

Parameter | Description
--------- | -------
firstName | First name
lastName | Last name
gender | "MALE" or "FEMALE"
birthday | Valid date in the format yyyy-MM-dd
phone | Phone number
