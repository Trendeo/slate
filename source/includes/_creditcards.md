# Saving credit cards

## Object definitions

> Credit card object:

```json
{
    "id": "card_1CCPcTAp1BNTIL1EImUQlKkL",
    "stripeCustomerId": "cus_CbcsmFilrpR57c",
    "cardLastDigits": "4242",
    "expirationMonth": 8,
    "expirationYear": 2019,
    "type": "Visa",
    "country": "US",
    "name": null,
    "usedByDefault": true,
    "lastUsedDate": null,
    "lastUsed": false,
    "valid": true
}
```

The credit cards API allows the clients to save Stripe credit card tokens to logged-in users. This enables us to offer a quick checkout with re-usable credit card informations.
The API expects Stripe credit card tokens, please refer to their documentations to understand how to create those on client side.

IMPORTANT: None of these API endpoints accept JWTs of a guest user, as a guest user is not allowed to store credit cards. An error will be thrown if you attempt to get, post or delete credit cards for guest users!

All endpoints are access-restricted, i.e. a valid user JWT is required.

Response objects have the following fields:

Parameter | Description
--------- | -------
id | Credit card ID which was created by Stripe
stripeCustomerId | Stripe-ID of their customer object which the card was attached to
cardLastDigits | Last 4 digits of the credit card, to show to the user when selecting from multiple cards
expirationMonth | Expiration month of the card
expirationYear | Expiration year of the card
type | Credit card type, e.g. "Visa"
country | Country code from where the credit card is
name | Usually "null", because we don't allow to name cards
usedByDefault | Whether the particular card is the default card of the user. This can be changed via this API. Only one card should have the value "true" among multiple cards
lastUsedDate | Date of last usage of this credit card
lastUsed | Boolean value whether the card was the last one used for a purchase (i.e. only one card should have the value "true" among multiple cards)
valid | Boolean value whether the card is still valid according to its expiration date. Invalid cards should not be shown to users any more, or should be flagged as expired properly

## Get credit cards for a user

Returns a dictionary of the saved credit cards for the logged-in user, with the token as key and the credit card JSON object (as defined above) as value.

> To get all saved cards of a logged-in user, use the following code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/user-restricted/payment/cards" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns a dictionary like this:

```json
{
    "tok_visa": {
        "id": "card_1CCPcTAp1BNTIL1EImUQlKkL",
        "stripeCustomerId": "cus_CbcsmFilrpR57c",
        "cardLastDigits": "4242",
        "expirationMonth": 8,
        "expirationYear": 2019,
        "type": "Visa",
        "country": "US",
        "name": null,
        "usedByDefault": true,
        "lastUsedDate": null,
        "lastUsed": false,
        "valid": true
    },
    "tok_visa_debit": {
        "id": "card_1CCPksAp1BNTIL1E4F0Bk7ic",
        "stripeCustomerId": "cus_Cbd00KDifBzLgm",
        "cardLastDigits": "5556",
        "expirationMonth": 8,
        "expirationYear": 2019,
        "type": "Visa",
        "country": "US",
        "name": null,
        "usedByDefault": false,
        "lastUsedDate": null,
        "lastUsed": false,
        "valid": true
    }
}
```

### HTTP Request

`GET https://apigw-dev.trendeo.com/user-restricted/payment/cards`


## Save a credit card

Allows to save a credit card to a logged-in user by providing the Stripe token.
This endpoint will then retrieve all required information from the Stripe API and will furthermore create a Stripe customer to attach the credit card to.
The API responds with the credit card JSON object (as defined above) of the newly created card.
An error will be thrown for the standard authentication errors, if the JWT belongs to a guest user, in case the token already exists for the particular logged-in user, and even in case a credit card with the same parameters (expiration date, type, last 4 digits) already exists for the particular user.

> To save a credit card, use the following code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/user-restricted/payment/cards/{token}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns the credit card JSON object of the new card:

```json
{
    "id": "card_1CCQ6LAp1BNTIL1Ec8am0lIz",
    "stripeCustomerId": "cus_CbdNuxzCfrArti",
    "cardLastDigits": "4444",
    "expirationMonth": 8,
    "expirationYear": 2019,
    "type": "MasterCard",
    "country": "US",
    "name": null,
    "usedByDefault": false,
    "lastUsedDate": null,
    "lastUsed": false,
    "valid": true
}
```

### HTTP Request

`POST https://apigw-dev.trendeo.com/user-restricted/payment/cards/{token}`

### Query Parameters

The request path must contain the token which you retrieved from Stripe. Please refer to their documentation to understand how to get one.


## Set a credit card as default

Allows to set one of the saved credit cards as default. Obviously you will receive an error in case the user does not have a credit card with the provided token.

> To set a credit card as default, use the following code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/user-restricted/payment/cards/{token}/default" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns all credit cards of the user in the JSON structure as defined above:

```json
{
    "tok_visa": {
        "id": "card_1CCPcTAp1BNTIL1EImUQlKkL",
        "stripeCustomerId": "cus_CbcsmFilrpR57c",
        "cardLastDigits": "4242",
        "expirationMonth": 8,
        "expirationYear": 2019,
        "type": "Visa",
        "country": "US",
        "name": null,
        "usedByDefault": true,
        "lastUsedDate": null,
        "lastUsed": false,
        "valid": true
    },
    "tok_visa_debit": {
        "id": "card_1CCPksAp1BNTIL1E4F0Bk7ic",
        "stripeCustomerId": "cus_Cbd00KDifBzLgm",
        "cardLastDigits": "5556",
        "expirationMonth": 8,
        "expirationYear": 2019,
        "type": "Visa",
        "country": "US",
        "name": null,
        "usedByDefault": false,
        "lastUsedDate": null,
        "lastUsed": false,
        "valid": true
    },
    "tok_mastercard": {
        "id": "card_1CCQ6LAp1BNTIL1Ec8am0lIz",
        "stripeCustomerId": "cus_CbdNuxzCfrArti",
        "cardLastDigits": "4444",
        "expirationMonth": 8,
        "expirationYear": 2019,
        "type": "MasterCard",
        "country": "US",
        "name": null,
        "usedByDefault": false,
        "lastUsedDate": null,
        "lastUsed": false,
        "valid": true
    }
}
```

### HTTP Request

`POST https://apigw-dev.trendeo.com/user-restricted/payment/cards/{token}/default`

### Query Parameters

The request path must contain the token which you retrieved from Stripe. Please refer to their documentation to understand how to get one.


## Delete a credit card

Allows to delete a credit card from the logged-in user. Obviously you will receive an error in case the user does not have a credit card with the provided token.

> To delete a credit card, use the following code:

```shell
curl -X DELETE "https://apigw-dev.trendeo.com/user-restricted/payment/cards/{token}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command responds with a 200 response code, and the simple String "Card was successfully removed"

### HTTP Request

`DELETE https://apigw-dev.trendeo.com/user-restricted/payment/cards/{token}`

### Query Parameters

The request path must contain the token which you retrieved from Stripe. Please refer to their documentation to understand how to get one.
