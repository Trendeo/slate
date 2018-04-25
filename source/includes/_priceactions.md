# Price Actions

## Object definitions

> A usual price review response looks as follows:

```json
{
    "id": 200,
    "merchantId": "diva-dames",
    "actionStart": "2018-01-15",
    "actionEnd": "2018-03-20",
    "addedStartDate": "2018-01-01",
    "blackPricesOnly": true,
    "title": "Test",
    "discountPercentage": 20,
    "createdAt": "2018-03-18 08:38:22",
    "updatedAt": "2018-03-18 08:38:22",
    "processedAt": null,
    "processedResult": null,
    "processedError": null,
    "filteredCategories": {
        "categories": [
            "test",
            "test2"
        ]
    },
    "excludedCategories": {
        "categories": [
            "test",
            "test2"
        ]
    },
    "active": false
}
```

> The object in the body for creating and updating needs to look as follows:

```json
{
    "discountPercentage": 30,
    "merchantId": "glamzam",
    "actionStart": "2018-01-15",
    "actionEnd": "2018-03-20",
    "addedStartDate": "2018-01-01",
    "blackPricesOnly": true,
    "title": "A price action",
    "filteredCategories": {
		    "categories": [
            "test",
			      "test2"
		    ]
    },
    "excludedCategories": {
        "categories": [
            "test",
            "test2"
        ]
    }
}
```

We allow our merchants to create "price actions". It gives the opportunity to apply a general percentage discount on all products, or just all products for specific categories, for a specified period of time. This API allows the creation, update and activation / deactivation of price actions.

All API endpoints are access restricted and need to be called with an appropriate merchant or admin JWT.

To create or update a price action, the following object needs to be included in the POST body - we will call this "price action request object" going forward:

Parameter | Required? | Description
--------- | ------- | -------
discountPercentage | yes | Numerical value for the percentage discount to apply. Must be between 1 and 99.
merchantId | yes | Merchant ID of the merchant owning the price action.
actionStart | yes | Start date in format yyyy-mm-dd.
actionEnd | yes | End date in format yyyy-mm-dd. Needs to be after start date
addedStartDate | no | This allows to restrict the products to which the particular price action is applied to products which were created (i.e. field "added") later than that given date. IMPORTANT: In case this date is not given, "1970-01-01" is returned (i.e. 0 ms since unix epoch), so you should show it somewhat differently to not confuse users.
blackPricesOnly | no | Whether the action should be only applied to non-discounted products, or to any. Default is false (i.e. applies to any product)
title | no | Title of the price action; not more than 255 characters.
filteredCategories | no | Object containing the field "categories", which just contains a string-array of all category IDs ("Women|Clothing|Dresses|...") to which the price action should be limited.
excludedCategories | no | Object containing the field "categories", which just contains a string-array of all category IDs ("Women|Clothing|Dresses|...") which should be excluded from this price action. It's basically the negation of "filteredCategories"

The responses from backend (from here on called "price action object") contain the following beyond the above-mentioned ones:

Parameter | Description
--------- | -------
id | Numerical ID of the action; primary key
createdAt | Creation date and time
updatedAt | Date and time of last update
processedAt | Date and time when the action has been processed last time
processedResult | Processing result; could be "NOT PROCESSED", "SUCCESSFUL", "ERROR"
processedError | Contains some error messages in case the processedResult is ERROR
active | true or false; whether the action is generally active or not. This does NOT mean whether it's currently going on or not, but just tells whether backend will process it once its time has come.


## Get all price actions of a merchant

Returns all price actions for a specific merchant, or all of them in case you are admin.

> To get all actions of a merchant, use this code:

```shell
curl -X GET "https://api-dev.trendeo.com/price-actions" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns an array of price action objects as defined above.

### HTTP Request

`GET https://api-dev.trendeo.com/price-actions`

## Get all active price actions of a merchant

Returns all active price actions for a specific merchant, or all of them in case you are admin.

> To get all active actions of a merchant, use this code:

```shell
curl -X GET "https://api-dev.trendeo.com/price-actions/active" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns an array of price action objects as defined above.

### HTTP Request

`GET https://api-dev.trendeo.com/price-actions/active`

## Get all inactive price actions of a merchant

Returns all inactive price actions for a specific merchant, or all of them in case you are admin.

> To get all inactive actions of a merchant, use this code:

```shell
curl -X GET "https://api-dev.trendeo.com/price-actions/inactive" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns an array of price action objects as defined above.

### HTTP Request

`GET https://api-dev.trendeo.com/price-actions/inactive`


## Get a specific price action

Returns a specific price action, or 404 in case not found or in case you requested a price action of a different merchant than yourself.

> To get a specific action of a merchant, use this code:

```shell
curl -X GET "https://api-dev.trendeo.com/price-actions/{id}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns the price action object with the given ID, as defined above.

### HTTP Request

`GET https://api-dev.trendeo.com/price-actions/{id}`

## Create a price action

Creates a price action for the merchant with the provided JWT, or for the merchantId in the POST body in case you are admin.
For a logged-in merchant, the parameter in the body is basically ignored, but should anyway be sent over.

> To create an action, use this code:

```shell
curl -X POST "https://api-dev.trendeo.com/price-actions" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..." \
  --data-raw "{ \"discountPercentage\": 30, \"merchantId\": \"glamzam\", \"actionStart\": \"2018-01-15\", \"actionEnd\": \"2018-03-20\", \"blackPricesOnly\": true, \"filteredCategories\": { \"categories\": [ \"test\", \"test2\" ] } }"
```

> The above command returns the created price action object, as defined above.

### HTTP Request

`POST https://api-dev.trendeo.com/price-actions`

## Update a price action

Updates the price action with the given ID.

> To update an action, use this code:

```shell
curl -X PATCH "https://api-dev.trendeo.com/price-actions/{id}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..." \
  --data-raw "{ \"discountPercentage\": 30, \"merchantId\": \"glamzam\", \"actionStart\": \"2018-01-15\", \"actionEnd\": \"2018-03-20\", \"blackPricesOnly\": true, \"filteredCategories\": { \"categories\": [ \"test\", \"test2\" ] } }"
```

> The above command returns the updated price action object, as defined above.

### HTTP Request

`PATCH https://api-dev.trendeo.com/price-actions/{id}`

## Activate a price action

Activates the price action with the given ID.

> To activate a action, use this code:

```shell
curl -X POST "https://api-dev.trendeo.com/price-actions/{id}/activate" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns the updated price action object, as defined above.

### HTTP Request

`POST https://api-dev.trendeo.com/price-actions/{id}/activate`

## Deactivate a price action

Deactivates the price action with the given ID.

> To deactivate a action, use this code:

```shell
curl -X POST "https://api-dev.trendeo.com/price-actions/{id}/deactivate" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns the updated price action object, as defined above.

### HTTP Request

`POST https://api-dev.trendeo.com/price-actions/{id}/deactivate`
