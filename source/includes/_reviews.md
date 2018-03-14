# Reviews

## Object definitions

> A usual product review response looks as follows:

```json
{
    "id": 1407,
    "productId": "818278d3-c011-35a3-95a6-1f43378d9835",
    "productSku": "Rosella_Khaki_12",
    "subOrderId": "UK170924-404664-27730",
    "reviewerName": "Larissa",
    "username": "larissawatts@gmail.com",
    "text": "Absolutely gorgeous! Fits perfectly",
    "rating": 5,
    "added": "2018-01-16T19:13:48.000+0000",
    "fit": "TRUE_TO_SIZE"
}
```

> A usual merchant review response looks very similar:

```json
{
    "id": 67,
    "merchantId": "glamzam",
    "subOrderId": "UK170920-404664-27566",
    "reviewerName": "Aaliyah",
    "username": "aaliyah_ross@hotmail.com",
    "text": "We were very pleased and would order from  Glamzam again, Awesome!",
    "rating": 5,
    "added": "2017-09-23T20:38:04.000+0000"
}
```

A user is allowed to leave reviews for merchants as well as for products. This is only possible after having a shipped order, to avoid receiving unreal reviews. A review consists of the following:

- Name of reviewer
- Rating from 1 to 5; leading to avg. star ratings shown in shop/app
- Fit (i.e. did the size fit as expected, or was smaller/larger than expected?); only applies to product reviews
- Review text (optional)

To avoid repeating the definitions in all sections, let's define the objects here once:
Both review types have the following attributes in common:

Parameter | Description
--------- | -------
id | Numerical ID of the review. primary key
subOrderId | ID of the subOrder for which the review was added
reviewerName | Name of the reviewer
username | Our username of the reviewing user
text | Review text, might be empty / missing
rating | rating from 1 to 5
added | Date and time when the review was added

Product reviews have the following specific parameters:

Parameter | Description
--------- | -------
productId | Hash-ID without market and color of the product
productSku | Supplier-SKU of the color-variation that was reviewed
fit | One of the textual values "SMALL", "TRUE_TO_SIZE", "LARGE"

Merchant reviews have the following specific parameters:

Parameter | Description
--------- | -------
merchantId | ID of the merchant that was reviewed

All API endpoints should receive and return JSON objects according to this specification.
When creating a review, all parameters except id, text and added are expected.

> A response for average merchant ratings looks as follows:

```json
{
    "merchantId": "glamzam",
    "avg": 5,
    "count": 4
}
```

> Average product ratings looks as follows:

```json
{
    "productId": "818278d3-c011-35a3-95a6-1f43378d9835",
    "productSku": "abc",
    "avg": 5,
    "count": 1
}
```

Furthermore, there are API endpoints which return average ratings for products or merchants. Those response objects have the following fields:

Parameter | Description
--------- | -------
productId | Hash-ID without market and color of the product
productSku | Supplier-SKU of the color-variation that was reviewed. Might be null or missing in case you request without it (i.e. for all variations)
avg | Float value with the average rating
count | Number of reviews that were left

For merchants, it contains the following fields:

Parameter | Description
--------- | -------
merchantId | ID of the merchant
avg | Float value with the average rating
count | Number of reviews that were left

> Fitting info objects for products look as follows:

```json
{
    "productId": "818278d3-c011-35a3-95a6-1f43378d9835",
    "productSku": "abc",
    "counts": {
        "SMALL": 0,
        "LARGE": 0,
        "TRUE_TO_SIZE": 1
    },
    "pct": {
        "SMALL": 0,
        "LARGE": 0,
        "TRUE_TO_SIZE": 1
    }
}
```

For products, there are also endpoints which return the "average fitting", indicated by the reviews that we got. The object has the following fields:

Parameter | Description
--------- | -------
productId | Hash-ID without market and color of the product
productSku | Supplier-SKU of the color-variation that was reviewed. Might be null or missing in case you request without it (i.e. for all variations)
counts | Contains the three possible fits SMALL, TRUE_TO_SIZE and LARGE, and the number of reviews that have that particular fitting-info set
pct | Same like counts, but as float-percentage. E.g. if 50% of reviews have selected SMALL, the value there will be 0.5.

> Object containing both types of reviews:

```json
{
    "merchantReviews": [
      merchantReviewObject1,
      merchantReviewObject2,
      ...
    ],
    "productsReviews": [
      productReviewObject1,
      productReviewObject2,
      ...
    ]
}
```

Additionally, there are endpoints which return both product and merchant reviews (e.g. get all reviews for a specific sub-order). In that case, a map is returned which contains the key "merchantReviews" and "productsReviews", which then contains just a list of the particular reviews.

## Sizing and pagination

The following 3 endpoints contain additional GET-parameters:

- GET https://apigw-dev.trendeo.com/reviews/merchants/{merchantId}
- GET https://apigw-dev.trendeo.com/reviews/products/{productId}
- GET https://apigw-dev.trendeo.com/reviews/products/{productId}/{supplierSku}

These endpoints understand the following two GET-parameters:

- page: integer of the exact page you want to get; 0 being the first one
- size: size of one page

It is required to either provide both of these parameters, or none.
If none of these parameters are provided, all reviews are returned.

## Get all reviews for a merchant

Returns all reviews that have been posted for a specific merchant.

> To get all reviews of a merchant, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/merchants/{merchantId}" \
  -H "Content-type: application/json"
```

> The above command returns an array of merchant reviews as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/merchants/{merchantId}`


## Get the average rating of a merchant

Returns the average ranking of a specific merchant.

> To get averages for a merchant, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/merchants/{merchantId}/avg" \
  -H "Content-type: application/json"
```

> The above command returns the average rating object as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/merchants/{merchantId}/avg`


## Get all reviews for a product (all variations)

Returns all reviews that have been posted for all color-variations of a product.

> To get all reviews of a product, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/products/{productId}" \
  -H "Content-type: application/json"
```

> The above command returns an array of product reviews as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/products/{productId}`


## Get the average rating of a product (all variations)

Returns the average ranking of all color-variations of a specific product.

> To get averages for a product, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/products/{productId}/avg" \
  -H "Content-type: application/json"
```

> The above command returns the average rating object as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/products/{productId}/avg`


## Get the average rating of a product (specific variation)

Returns the average ranking of a specific color variation of a specific product.

> To get averages for a product variation, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/products/{productId}/{productSku}/avg" \
  -H "Content-type: application/json"
```

> The above command returns the average rating object as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/products/{productId}/{productSku}/avg`


## Get the average fitting info of a product (all variations)

Returns the average fitting info of all color-variations of a specific product.

> To get averages for a product, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/products/{productId}/fitInfo" \
  -H "Content-type: application/json"
```

> The above command returns the average fitting info object as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/products/{productId}/fitInfo`


## Get the average fitting info of a product (specific variation)

Returns the average fitting info of a specific color variation of a specific product.

> To get averages for a product variation, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/products/{productId}/{productSku}/fitInfo" \
  -H "Content-type: application/json"
```

> The above command returns the average fitting info object as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/products/{productId}/{productSku}/fitInfo`


## Get all reviews for a product (specific variation)

Returns all reviews that have been posted for a specific color-variation of a product.

> To get all reviews of a product, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/reviews/products/{productId}/{supplierSku}" \
  -H "Content-type: application/json"
```

> The above command returns an array of product reviews as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/reviews/products/{productId}/{supplierSku}`


## Get all merchant reviews by logged-in user

Returns all merchant reviews that have been posted by the logged-in user.

> To get all reviews of a merchant for the logged-in user, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/user_restricted/reviews/merchants" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns an array of merchant reviews as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/user_restricted/reviews/merchants`


## Get all reviews for a specific sub-order

Returns all merchant and product reviews that have been posted in context of a specific sub-order.

> To get all reviews in context of a specific sub-order, use this code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/orders/{subOrderId}" \
  -H "Content-type: application/json"
```

> The above command returns a dictionary of both review types as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/orders/{subOrderId}`


## Get all reviews done by logged-in user

Returns all merchant and product reviews that have been posted by a specific user.

> To get all reviews of a specific user, use the following code:

```shell
curl -X GET "https://apigw-dev.trendeo.com/user_restricted/reviews/user" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns a dictionary of both review types as defined above

### HTTP Request

`GET https://apigw-dev.trendeo.com/user_restricted/reviews/user`


## Post a product review

Creates a product review by the logged-in user.

> To create product review, use the following code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/user_restricted/reviews/products" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
  --data-raw "{\"productId\":\"818278d3-c011-35a3-95a6-1f43378d9835\",\"productSku\":\"Rosella_Khaki_14\",\"text\":\"Cool product\",\"reviewerName\":\"Max\",\"rating\":5,\"subOrderId\":\"UK170804-314340-25659\",\"fit\":\"SMALL\"}"
```

The "text" field is the only optional field.
This endpoint will return the resulting product review as defined above.

### HTTP Request

`POST https://apigw-dev.trendeo.com/user_restricted/reviews/products`


## Post a merchant review

Creates a merchant review by the logged-in user.

> To create merchant review, use the following code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/user_restricted/reviews/merchants" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
  --data-raw "{\"subOrderId\":\"UK170804-314340-25659\",\"text\":\"Cool merchant\",\"reviewerName\":\"Max\",\"rating\":5}"
```

The "text" field is the only optional field. The merchantId is not included here, because it will be taken from the sub-order.
This endpoint will return the resulting merchant review as defined above.

### HTTP Request

`POST https://apigw-dev.trendeo.com/user_restricted/reviews/merchants`
