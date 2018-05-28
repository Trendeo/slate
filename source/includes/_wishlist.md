# Wishlist

## Object definitions

> A wishlist response contains an error of "reduced product objects", as they were already defined in the catalog endpoints:

```json
[
    {
        "id": "uk_8cb7a4d4-4569-3f15-a003-e4d47646cd61_rose",
        "slug": "uk_8cb7a4d4-4569-3f15-a003-e4d47646cd61_rose",
        "images": [
            "jadore-you/aHR0cDovL2NkbjYuYmlnY29tbWVyY2UuY29tL3MteXkzMDgvcHJvZHVjdHMvMjk0Mi9pbWFnZXMvMTE5NjQvSU1HXzA1NjdfXzYwOTkwLjE1MDM2Njg0NDEuMTI4MC4xMjgwLmpwZw.jpg"
        ],
        "mainImage": "jadore-you/aHR0cDovL2NkbjYuYmlnY29tbWVyY2UuY29tL3MteXkzMDgvcHJvZHVjdHMvMjk0Mi9pbWFnZXMvMTE5NjQvSU1HXzA1NjdfXzYwOTkwLjE1MDM2Njg0NDEuMTI4MC4xMjgwLmpwZw.jpg",
        "price": 24.5,
        "originalPrice": 24.5,
        "discountPct": 0,
        "title": "Waterfall Faux Suede Coat With Belt in Dusty Pink",
        "merchantId": "jadore-you"
    }
]
```

> The object in the body for adding or removing items from a wishlist looks as follows:

```json
{
    "market": "UK",
	   "entries": [
		     {
			        "productId": "a3f3de23-86ef-3b4f-a943-79ab594effc9",
			        "productSku": "Sylvie_Black_ML"
		     }
	   ]
}
```

The user may add or remove items from her/his wishlist. Both endpoints (for adding and removing) expect the same JSON body, containing the market and an array of simple objects containing just the productId (without market or color) and the merchant-provided SKU of any variant of the product.

All API endpoints are access restricted and need to be called with an appropriate user JWT.

To add or remove an item, the following object needs to be included in the POST body - we will call this "wishlist change request object" going forward:

Parameter | Required? | Description
--------- | ------- | -------
market | yes | The ID of the market the user is currently looking at.
entries | yes | An array of objects containing the fields "productId" (dynamo ID without color or market) and "productSku" (an sku of any variant of the product that should be added. This is not an optimal implementation, but for now it's like that...). You can provide many of these objects to add or remove many items at once.

The responses from backend has already been defined in the catalog object definitions ("reduced product object").

## Get a wishlist

Returns all products that are available in the given market (i.e. the response will obviously look different for different markets, although the list in DB is the same).
The path contains the market (2-symbol market code in lower-case) and can contain the parameters "page" (0 being the first) and/or "pageSize" (number of products to be returned). Both query string parameters are optional and are defaulting to 0 and 48 respectively.

> To get the wishlist of a user, use this code:

```shell
curl -X GET "https://api-dev.trendeo.com/user-restricted/wishlist/{market}?page=0&pageSize=48" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns an array of reduced product objects as defined above.

### HTTP Request

`GET https://api-dev.trendeo.com/user-restricted/wishlist/{market}`

## Get only product IDs in wishlist

The endpoint returns just a JSON array of product IDs (without market) which the logged-in user has in her/his wishlist.
It was created to save bandwidth during catalog rendering, which needs to show which products are in the wishlist and which not.
The IDs have the structure {product-id}_{color-slug}, i.e. the regular long product ID format just without the market.

> To get the product IDs in the wishlist of a user, use this code:

```shell
curl -X GET "https://api-dev.trendeo.com/user-restricted/wishlist/{market}/product-ids-only" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns an array of product-id-strings in format {product-id}_{color-slug}.

### HTTP Request

`GET https://api-dev.trendeo.com/user-restricted/wishlist/{market}/product-ids-only`

## Clear a wishlist

Removes all products from the wishlist of a user.

> To remove all items from a user wishlist, use this code:

```shell
curl -X DELETE "https://api-dev.trendeo.com/user-restricted/wishlist" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..."
```

> The above command returns status code 200 without a body.

### HTTP Request

`DELETE https://api-dev.trendeo.com/user-restricted/wishlist`

## Add items

Adds items to a user wishlist.
This endpoint will return an error BAD GATEWAY in case the product could not be found or the provided productSku can not be found among the variants of the given productId.

> To add items to a user wishlist, use this code:

```shell
curl -X POST "https://api-dev.trendeo.com/user-restricted/wishlist/add" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..." \
  --data-raw "{ \"market\": \"UK\", \"entries\": [ { \"productId\": \"a3f3de23-86ef-3b4f-a943-79ab594effc9\", \"productSku\": \"Sylvie_Black_ML\" } ] }"
```

> The above command returns status code 200 without a body.

### HTTP Request

`POST https://api-dev.trendeo.com/user-restricted/wishlist/add`

## Remove items

Removes items from a user wishlist.
This endpoint will return an error BAD GATEWAY in case the product could not be found or the provided productSku can not be found among the variants of the given productId.
No error will be thrown in case the user actually does not have the given products in her/his wishlist.

> To remove items from a user wishlist, use this code:

```shell
curl -X POST "https://api-dev.trendeo.com/user-restricted/wishlist/remove" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc..." \
  --data-raw "{ \"market\": \"UK\", \"entries\": [ { \"productId\": \"a3f3de23-86ef-3b4f-a943-79ab594effc9\", \"productSku\": \"Sylvie_Black_ML\" } ] }"
```

> The above command returns status code 200 without a body.

### HTTP Request

`POST https://api-dev.trendeo.com/user-restricted/wishlist/remove`
