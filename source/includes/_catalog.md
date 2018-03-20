# Catalog

## Object definitions

> Reduced product object:

```json
{
    "id": "uk_f37262ea-563e-3659-9579-ea24c1c25e0e_black",
    "slug": "uk_f37262ea-563e-3659-9579-ea24c1c25e0e_black",
    "images": [
        "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvcS81Mzkvd2ViX0RTQ184MzE0X18wNDU4OC5qcGc.jpg",
        "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvei8wMzkvd2ViX0RTQ184MzE3X181NDM2Ni5qcGc.jpg",
        "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvYi83NTEvd2ViX0RTQ184MzI0X180MjQzMi5qcGc.jpg",
        "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvdi82Mjkvd2ViX0RTQ184MzM0X184NTcwOC5qcGc.jpg"
    ],
    "mainImage": "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvcS81Mzkvd2ViX0RTQ184MzE0X18wNDU4OC5qcGc.jpg",
    "price": 55,
    "originalPrice": 55,
    "discountPct": 0,
    "title": "Nola Black Backless Maxi Grecian Dress",
    "merchantId": "nazz-collection"
}
```

> A full product object looks like this:

```json
{  
    "id":"de_e8e5500f-57f0-3946-8860-751f989efc51_brown",
    "merchantId":"solewish",
    "title":"Tabitha Brown Riding Boots",
    "description":"Material: Synthetic",
    "fitType":"",
    "categories":[  
        "Women|Shoes|Boots|Knee",
        "Women|Shoes|Boots|Flat"
    ],
    "images":[  
        "solewish/aHR0cDovL3NvbGV3aXNoLmNvbS9tZWRpYS9jYXRhbG9nL3Byb2R1Y3QvY2FjaGUvMS9pbWFnZS85ZGY3OGVhYjMzNTI1ZDA4ZDZlNWZiOGQyNzEzNmU5NS9yL2kvcmlkaW5nYm9vdHNicm93bmh4Ny5qcGc.jpg",
        "solewish/aHR0cDovL3NvbGV3aXNoLmNvbS9tZWRpYS9jYXRhbG9nL3Byb2R1Y3QvY2FjaGUvMS9pbWFnZS85ZGY3OGVhYjMzNTI1ZDA4ZDZlNWZiOGQyNzEzNmU5NS9kL3MvZHNjMDUxMzQuanBn.jpg",
        "solewish/aHR0cDovL3NvbGV3aXNoLmNvbS9tZWRpYS9jYXRhbG9nL3Byb2R1Y3QvY2FjaGUvMS9pbWFnZS85ZGY3OGVhYjMzNTI1ZDA4ZDZlNWZiOGQyNzEzNmU5NS9kL3MvZHNjMDUxMzcuanBn.jpg"
    ],
    "mainImage":"solewish/aHR0cDovL3NvbGV3aXNoLmNvbS9tZWRpYS9jYXRhbG9nL3Byb2R1Y3QvY2FjaGUvMS9pbWFnZS85ZGY3OGVhYjMzNTI1ZDA4ZDZlNWZiOGQyNzEzNmU5NS9yL2kvcmlkaW5nYm9vdHNicm93bmh4Ny5qcGc.jpg",
    "color":"Brown",
    "otherColors":[  
        {
            "color": "green",
            "slug": "uk_abd_green"
        }
    ],
    "variations":[  
        {  
            "discountPct":41.0,
            "originalPrice":41.49,
            "price":24.49,
            "size":"3",
            "sizeSelect":"UK 3 (EU 36)",
            "sku":"8761",
            "stock":0
        },
        {  
            "discountPct":41.0,
            "originalPrice":41.49,
            "price":24.49,
            "size":"4",
            "sizeSelect":"UK 4 (EU 37)",
            "sku":"8762",
            "stock":0
        },
        {  
            "discountPct":41.0,
            "originalPrice":41.49,
            "price":24.49,
            "size":"5",
            "sizeSelect":"UK 5 (EU 38)",
            "sku":"8763",
            "stock":0
        },
        {  
            "discountPct":41.0,
            "originalPrice":41.49,
            "price":24.49,
            "size":"6",
            "sizeSelect":"UK 6 (EU 39)",
            "sku":"8764",
            "stock":0
        },
        {  
            "discountPct":41.0,
            "originalPrice":41.49,
            "price":24.49,
            "size":"7",
            "sizeSelect":"UK 7 (EU 40)",
            "sku":"8765",
            "stock":0
        }
    ],
    "materials":[  
        {  
            "name":"Synthetic",
            "percentage":100
        }
    ],
    "discountPct":41.0,
    "price":24.49,
    "originalPrice":41.49,
    "totalStock":0,
    "status":"DISABLED"
}
```

The Catalog APIs allow access to our ElasticSearch indexes for the following purposes:

- Make requests for catalog pages, incl. filtering by merchant, color, category, tags, sizes, search-terms, price ranges
- Get product-to-product recommendations based on ElasticSearch More-like-this feature
- Get an individual product for rendering the product page / screen

There are two types of objects that are returned by these endpoints. The first is more extensive, contains more information and is used for the product page. The second one is more concise to be displayed in catalog or as recommendation. We will call these objects from now on "reduced product object" and "full product object".

The reduced product object has the following structure:

Parameter | Description
--------- | -------
id | Market- and variant-color specific product id
slug | Product slug to be used in URLs etc.
images | List of all product images, sorted by "importance" (i.e. should be rendered in that order)
mainImage | Main image to be used as catalog image and first image to be shown on product page / screen
price | Current sales price. Float
originalPrice | Price before discount. Should be always >= price. Float
discountPct | Percentage of discount (i.e. (price / originalPrice - 1) * 100). Float
title | Title of the product
merchantId | Merchant for that product

The full product object contains the following fields:

Parameter | Description
--------- | -------
id | Market- and variant-color specific product id
images | List of all product images, sorted by "importance" (i.e. should be rendered in that order)
mainImage | Main image to be used as catalog image and first image to be shown on product page / screen
price | Current sales price. Float
originalPrice | Price before discount. Should be always >= price. Float
discountPct | Percentage of discount (i.e. (price / originalPrice - 1) * 100). Float
title | Title of the product
description | Description of the product
merchantId | Merchant for that product
fitType | Some manually maintained fitType - usually not used anywhere...
categories | List of categories
color | Color of the particular variant
otherColors | List of "otherColor" objects - each object contains the field "color" and "slug"
variations | List of all sizes; each size-object contains the fields price, originalPrice, discountPct, size, sizeSelect, sku and stock. Prices are for now irrelevant, just added in case we will have size-specific prices some day. sizeSelect is usually used in the size choice on product page, while size is a harmonised value to be used in filters
materials | List of all materials and their percentages. Each material-object contains the fields "name" and "percentage". Percentage is full integer between 0 and 100.
totalStock | Total stock of all sizes, should be the sum of all stock-values of all sizes
status | PUBLIC when online, DISABLED when not


## Get catalog pages

Allows to request catalog product lists by various filters.

> Following request is an example for a catalog query:

```shell
curl -X POST "https://apigw-dev.trendeo.com/catalog/search" \
  -H "Content-type: application/json" \
  --data-raw "{ \"searchText\": \"dress\", \"sortField\": \"POPULARITY\", \"sortDirection\": \"DESC\", \"market\": \"UK\", \"filteredSizes\": [ \"S\", \"M\" ], \"filteredMerchants\": [ \"nazz-collection\", \"glamzam\", \"pretty-and-party\" ], \"filteredCategories\": [ \"Women|Clothing|Dresses\" ], \"filteredTags\": [ \"embroideredpieces\" ], \"filteredColors\": [ \"red\", \"rose\", \"black\" ], \"minPrice\": 30, \"maxPrice\": 55, \"minDiscount\": 10, \"maxDiscount\": 25, \"page\": 0, \"pageSize\": 10, \"aggregationsIncluded\": true, \"publicOnly\": true }"
```

> The above command returns a dictionary like this:

```json
{
    "products": [
        {
            "id": "uk_68fbb939-1389-35f8-81e1-c71fd43a31e4_black",
            "slug": "uk_68fbb939-1389-35f8-81e1-c71fd43a31e4_black",
            "images": [
                "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvZy82MDQvMTk5M18oNSlfXzQ2NDE4LmpwZw.jpg",
                "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvbi81MDAvMTk5M18oNylfXzAwOTQ2LmpwZw.jpg",
                "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvcS83NDIvMTk5M18oNilfXzQ5NDQyLkpQRw.JPG",
                "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvZy81MzgvMTk5M18oOClfXzMyNzY1LmpwZw.jpg"
            ],
            "mainImage": "nazz-collection/aHR0cDovL3d3dy5uYXp6Y29sbGVjdGlvbi5jb20vcHJvZHVjdF9pbWFnZXMvZy82MDQvMTk5M18oNSlfXzQ2NDE4LmpwZw.jpg",
            "price": 35,
            "originalPrice": 35,
            "discountPct": 0,
            "title": "Karla Black Plunge Scalloped Lace Embroidery Midi Dress",
            "merchantId": "nazz-collection"
        }
    ],
    "aggregations": {
        "minprice": 35,
        "maxprice": 35,
        "sizes": {
            "s": 2,
            "xs": 1,
            "l": 1,
            "m": 2
        },
        "categories": {
            "Women": 1,
            "Women|Clothing|Dresses": 1,
            "Women|Clothing": 1,
            "Women|Clothing|Dresses|Evening": 1,
            "Women|Clothing|Dresses|Midi": 1
        },
        "merchants": {
            "nazz-collection": 1
        },
        "colors": {
            "black": 1
        }
    },
    "totalResults": 1
}
```

### HTTP Request

`POST https://apigw-dev.trendeo.com/reviews/merchants/{merchantId}`

### Query Parameters

The request should be done with POST method and can contain the following fields as JSON body:

Parameter | Required? | Description
--------- | ------- | -----------
market | yes | Market catalog to search in
pageSize | yes | Number of products per page.
searchText | no | Free-text search input
page | no | Page you would like to receive. By default returns the first page (0).
minPrice | no | Minimum price to filter. 0.01 by default
maxPrice | no | Maximum price to filter. No maximum price by default
minDiscount | no | The minimum discount in % that the returned products have
maxDiscount | no | The maximum discount in % that the returned products have
publicOnly | no | "true" or "false"; determines whether you will get only products with status "PUBLIC", or all. The API returns also non-PUBLIC products by default
aggregationsIncluded | no | Do you want the aggregations for filters to be included or not (true or false). Not included by default
sortField | no | Field to sort by. Can be one of "POPULARITY", "CREATED" or "PRICE". Default is "POPULARITY"
sortDirection | no | Either "DESC" or "ASC". "DESC" by default
filteredMerchants | no | String-array of merchant IDs to filter. Case-insensitive.
filteredColors | no | String-array of colors to filter. Case-insensitive.
filteredTags | no | String-array of tags to filter. Case-insensitive.
filteredSizes | no | String-array of sizes to filter. Case-insensitive.
filteredCategories | no | String-array of categories in the usual "Women|Clothing|Dresses|..." format to filter. Case-insensitive.

### Response
The API responds with a JSON object with the following three fields:

- "products": Contains an array of reduced product objects that matches your given criteria
- "aggregations": Contains an object with the following fields: "minprice" containing the float-value of the minimum price of the products matching those criteria; "maxprice" containing the float-value of maximum price; "sizes" contains a dictionary of sizes and int-value of occurence; "categories", "merchants" and "colors" the same for their particular attribute. Be careful when reading those objects: If for example no products were found, the aggregations field can be just an empty object or might not contain one or several of these keys.
- "totalResults": int-value of total products matching the given criteria


## Get product-to-product recommendations

Allows to find similar products to a given product to show as recommendations.

> Following request is an example for a product-to-product recommendation query:

```shell
curl -X POST "https://apigw-dev.trendeo.com/catalog/recommendations/product-to-product" \
  -H "Content-type: application/json" \
  --data-raw "{ \"id\": \"uk_0dc4df8f-dc95-372d-bd45-2f9aa5e0d8e5_stone\", \"market\": \"UK\", \"size\": 10, \"matchCoefficient\": 20, \"crossSell\": false }"
```

> The above command returns an array of reduced product objects like this:

```json
[
    {
        "id": "uk_08f5d102-088d-41ab-9d78-7c4711e00d1c_olive",
        "slug": "uk_08f5d102-088d-41ab-9d78-7c4711e00d1c_olive",
        "images": [
            "itsin-fashion/41aefbmtdvl.jpg"
        ],
        "mainImage": "itsin-fashion/41aefbmtdvl.jpg",
        "price": 11.95,
        "originalPrice": 11.95,
        "discountPct": 0,
        "title": "Celebrity Turtle Neck Long Sleeve Ribbed Bodycon Dress",
        "merchantId": "itsin-fashion"
    },
    {
        "id": "uk_08f5d102-088d-41ab-9d78-7c4711e00d1c_black",
        "slug": "uk_08f5d102-088d-41ab-9d78-7c4711e00d1c_black",
        "images": [
            "itsin-fashion/41renc0a1vl.jpg"
        ],
        "mainImage": "itsin-fashion/41renc0a1vl.jpg",
        "price": 11.95,
        "originalPrice": 11.95,
        "discountPct": 0,
        "title": "Celebrity Turtle Neck Long Sleeve Ribbed Bodycon Dress",
        "merchantId": "itsin-fashion"
    }
]
```

### HTTP Request

`POST https://apigw-dev.trendeo.com/catalog/recommendations/product-to-product`

### Query Parameters

The request should be done with POST method and can contain the following fields as JSON body:

Parameter | Required? | Description
--------- | ------- | -----------
market | yes | Market catalog to search in
id | yes | market-color specific ID of the product-variation to find recommendations for
size | no | Number of products to return. 4 by default.
filteredMerchants | no | String-array of merchant IDs to filter. Case-insensitive.
filteredCategories | no | String-array of categories in the usual "Women|Clothing|Dresses|..." format to filter. Case-insensitive.
matchCoefficient | no | Elastic-internal coefficient to define how strongly the documents need to match with the given one to be included in the result. The lower this value is, the more recommendations you will probably get, but the less they might match with the product to compare to. Can be between 1 and 100. Defaults to 80.
crossSell | no | Boolean value (true or false). Default is true. This value means the preference of whether products should be found in the same category. It's not a guarantee, but if set to true, Elastic will give category match a stronger influence.

### Response
The API responds with a JSON array of reduced product objects that are more or less similar to the given product. Important: The number of results might be lower than given in the "size" parameter, in case Elastic just couldn't find so many products as you wanted.


## Get a paticular product

Allows to get full elastic information about one specific product.

> Following request is an example for a product-to-product recommendation query:

```shell
curl -X GET "https://apigw-dev.trendeo.com/catalog/product/{market}/{productId_color}" \
  -H "Content-type: application/json"
```

> The above command filled with market and product ID returns the full product object:

```json
{  
    "id":"uk_aeb3ddc2-aecc-3976-8f25-f49bff8aef61_black",
    "merchantId":"jadore-you",
    "title":"Ring Detail Skinny Denim Raw Hem Jeans",
    "description":"Measurements Size 10 : Length 36\", Width 30\" Model's height: 5'10'' Material Main : 70% Cotton 27% Polyester 3% Elastane",
    "fitType":"",
    "categories":[  
        "Women|Clothing|Jeans|Skinny fit",
        "Women|Clothing|Jeans|Cropped"
    ],
    "images":[  
        "jadore-you/aHR0cDovL2NkbjYuYmlnY29tbWVyY2UuY29tL3MteXkzMDgvaW1hZ2VzL3N0ZW5jaWwvMTI4MHgxMjgwL3Byb2R1Y3RzLzI3ODMvMTEzNTUvSU1HXzg5NzlfXzUyMTIyLjE1MDExNzMwMzMuanBn.jpg",
        "jadore-you/aHR0cDovL2NkbjYuYmlnY29tbWVyY2UuY29tL3MteXkzMDgvaW1hZ2VzL3N0ZW5jaWwvMTI4MHgxMjgwL3Byb2R1Y3RzLzI3ODMvMTEzNTQvSU1HXzg5ODFfXzg2NDEzLjE1MDExNzMwMzIuanBn.jpg"
    ],
    "mainImage":"jadore-you/aHR0cDovL2NkbjYuYmlnY29tbWVyY2UuY29tL3MteXkzMDgvaW1hZ2VzL3N0ZW5jaWwvMTI4MHgxMjgwL3Byb2R1Y3RzLzI3ODMvMTEzNTUvSU1HXzg5NzlfXzUyMTIyLjE1MDExNzMwMzMuanBn.jpg",
    "color":"Black",
    "otherColors":[  

    ],
    "variations":[  
        {  
            "discountPct":0.0,
            "originalPrice":21.5,
            "price":21.5,
            "size":"S",
            "sizeSelect":"UK 10",
            "sku":"2783_uk_10",
            "stock":5
        },
        {  
            "discountPct":0.0,
            "originalPrice":21.5,
            "price":21.5,
            "size":"M",
            "sizeSelect":"UK 12",
            "sku":"2783_uk_12",
            "stock":5
        },
        {  
            "discountPct":0.0,
            "originalPrice":21.5,
            "price":21.5,
            "size":"M",
            "sizeSelect":"UK 14",
            "sku":"2783_uk_14",
            "stock":5
        },
        {  
            "discountPct":0.0,
            "originalPrice":21.5,
            "price":21.5,
            "size":"XS",
            "sizeSelect":"UK 6",
            "sku":"2783_uk_6",
            "stock":5
        },
        {  
            "discountPct":0.0,
            "originalPrice":21.5,
            "price":21.5,
            "size":"S",
            "sizeSelect":"UK 8",
            "sku":"2783_uk_8",
            "stock":5
        }
    ],
    "materials":[  
        {  
            "name":"Cotton",
            "percentage":70
        },
        {  
            "name":"Polyester",
            "percentage":27
        },
        {  
            "name":"Elastane",
            "percentage":3
        }
    ],
    "discountPct":0.0,
    "price":21.5,
    "originalPrice":21.5,
    "totalStock":25,
    "status":"PUBLIC"
}
```

### HTTP Request

`GET https://apigw-dev.trendeo.com/catalog/product/{market}/{productId_color}`

### Query Parameters

The request should be done with GET method and contain the following path elements:

Parameter | Required? | Description
--------- | ------- | -----------
market | yes | Market version of the product to find
productId_color | yes | E.g. "aeb3ddc2-aecc-3976-8f25-f49bff8aef61_black"

### Response
The API responds with the full product object as defined above, or 404 in case the product could not be found.
The API will also return DISABLED products, so a successful lookup does NOT mean that the product should be shown to users!
