---
title: Trendeo Backend API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

includes:
  - errors

search: true
---

# Introduction

Welcome to the reference documentation of the backend APIs for trendeo.com! This documentation should provide all details you need to work with the backend. In case anything doesn't work as you expect or you have some suggestions for improvement, please shoot to dominik@trendeo.com!

# API Hosts

There are two different API hosts for all backend APIs - one for staging and one for production. **https://apigw.trendeo.com** works for production, and **https://apigw-dev.trendeo.com** for staging - choose wisely ;)
All API endpoints below are referring to the staging environment. Please always use HTTPS, HTTP might get deprecated at some point in the future.

# Passing parameters

All POST endpoints expect JSON as body. Please always include "application/json" in the Content-type header.

All GET endpoints expect parameters as regular query-string parameters.

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
A user can only log in or request a password reset in case there has been a regular sign-up with email/password before. In case the particular user has been created via Facebook sign-up and then attempts to log in with regular email/password, or attempts to reset the password, these requests are denied. The user first has to go through the regular sign-up process to use that particular account in that way.
The API provides comprehensive feedback about reasons for denying requests; it's up to the API user what to present to the user and how.

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
  "refreshToken" : "abc..."
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

### Response

A successful request gets a new JWT and refreshToken in return. This means, that a potentially existing refreshToken is not valid any more. The JWT can be used for all access-restricted API endpoints for a small amount of time (several hours), while the refreshToken can be used for an extended period of time (multiple weeks) to obtain a new JWT. Keep the refreshToken very safe and secure, and don't use it anywhere else than for getting a new JWT (API endpoint for that is described further below).

Parameter | Description
--------- | -------
jwt | Your fresh and new JWT for future requests.
refreshToken | Token for getting a new JWT once its expired.

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
  "refreshToken" : "abc..."
}
```

#### HTTP Request

`POST https://apigw-dev.trendeo.com/authentication/social/facebook`

#### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
market | yes | The 2-symbol market code from which the user is logging in / signing up.
accessToken | yes | The Facebook access token obtained from the redirect. Will be used to access the user profile and get all required details to create a user in our database, if one does not exist yet.

#### Response

The API responds with the same structure you also see in the regular login (a jwt and a refresh token).

Parameter | Description
--------- | -------
jwt | Your fresh and new JWT for future requests.
refreshToken | Token for getting a new JWT once its expired.

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
  "refreshToken" : "abc..."
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
  --data-raw "{\"password\":\"test-password\",\"resetPasswordToken\":\"Af33kd...\",\"usernameBase64\",\"AE302...\"}"
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
  "refreshToken" : "abc..."
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

## Response

This API endpoint creates the user and logs her/him in immediately. Hence, the response contains the short-lived JWT as well as the long-lived refresh-token.
If newsletterAllowed is set to true, the user is subscribed to the mailchimp list of the given market (synchronously).

Parameter | Description
--------- | -------
jwt | A new short-lived JWT for further API communication.
refreshToken | A long-lived refresh-token to renew the jwt when it expired.
