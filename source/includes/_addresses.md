# User addresses

Users can have as many addresses as they like. Every address receives an alphanumerical ID at time of creation. Addresses can be "primary" in general, and primary for either delivery or billing (or both). All address API endpoints require a valid JWT, i.e. are access restricted.

## Create an address

> To create an address, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/user/address" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc" \
  --data-raw "{
  	\"firstName\": \"Dominik\",
  	\"lastName\": \"Picker\",
  	\"email\": \"dominik@trendeo.com\",
  	\"market\": \"UK\",
  	\"street1\": \"Obernstraße 39\",
  	\"street2\": \"Appartment 11\",
  	\"street3\": \"Left entrance\",
  	\"city\": \"London\",
  	\"postalCode\": \"N1 6DR\",
  	\"phone\": \"+7 915 4334358\",
  	\"newsletterAllowed\": true,
  	\"primary\": false,
  	\"primaryBilling\": false,
  	\"primaryDelivery\": true
  \}"
```

> The above command returns the newly created address as JSON structure:

```json
{
    "id": "JQmPj7NG9",
    "firstName": "Dominik",
    "lastName": "Picker",
    "companyName": null,
    "street1": "Obernstraße 39",
    "street2": "Appartment 11",
    "street3": "Left entrance",
    "city": "London",
    "postalCode": "N1 6DR",
    "state": null,
    "newsletter": true,
    "country": "UK",
    "phone": "+7 915 4334358",
    "email": "dominik@trendeo.com",
    "addressType": "RESIDENTIAL",
    "primary": false,
    "primaryDelivery": true,
    "primaryBilling": false
}
```

### HTTP Request

`POST https://apigw-dev.trendeo.com/user/address`

### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
firstName | yes | First name
lastName | yes | Last name
email | yes | Email address
market | yes | Must be one of our active market IDs
street1 | yes | Address field 1
street2 | no | Address field 2
street3 | no | Address field 3
city | yes | City
postalCode | yes | Zip code; no validation done in backend
phone | yes | Phone number; no validation done in backend (e.g. allow numerical only or whatever)
newsletterAllowed | no | Is the user fine with being subscribed? Either true or false, false is default. The API does NOT subscribe the user rightaway, this only happens when an order is placed.
primary | no | Is this the primary address of the user? True or false, false is default
primaryDelivery | no | Is this the primary delivery address of the user? True or false, false is default
primaryBilling | no | Is this the primary billing address of the user? True or false, false is default

### Response

A successful request creates the address with a newly created address ID. In case any of the "primary" fields is set to "true", any other address having that field set to "true" is turned into "false", to make sure that there is just one primary address at any point in time.

Parameter | Description
--------- | -------
id | Newly created alphanumerical ID of the new address
firstName | First name
lastName | Last name
companyName | Deprecated field, should be always null
street1 | Address field 1
street2 | Address field 2
street3 | Address field 3
city | City
postalCode | Zip code
state | Deprecated field, should be always null
newsletter | Is the user fine with being subscribed with the email address of that address?
market | Market which the address is assigned to
phone | Phone number
email | Email address
addressType | Deprecated field, should always be "RESIDENTIAL"
primary | Is this the primary address of the user?
primaryDelivery | Is this the primary delivery address of the user?
primaryBilling | Is this the primary billing address of the user?

## Update OR create an address

> To update an address, use this code:

```shell
curl -X PATCH "https://apigw-dev.trendeo.com/user/address/{add_address_id_here}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc" \
  --data-raw "{
  	\"firstName\": \"Dominik\",
  	\"lastName\": \"Picker\",
  	\"email\": \"dominik@trendeo.com\",
  	\"market\": \"UK\",
  	\"street1\": \"Obernstraße 39\",
  	\"street2\": \"Appartment 11\",
  	\"street3\": \"Left entrance\",
  	\"city\": \"London\",
  	\"postalCode\": \"N1 6DR\",
  	\"phone\": \"+7 915 4334358\",
  	\"newsletterAllowed\": true,
  	\"primary\": false,
  	\"primaryBilling\": false,
  	\"primaryDelivery\": true
  \}"
```

> The above command returns the updated address as JSON structure:

```json
{
    "id": "JQmPj7NG9",
    "firstName": "Dominik",
    "lastName": "Picker",
    "companyName": null,
    "street1": "Obernstraße 39",
    "street2": "Appartment 11",
    "street3": "Left entrance",
    "city": "London",
    "postalCode": "N1 6DR",
    "state": null,
    "newsletter": true,
    "country": "UK",
    "phone": "+7 915 4334358",
    "email": "dominik@trendeo.com",
    "addressType": "RESIDENTIAL",
    "primary": false,
    "primaryDelivery": true,
    "primaryBilling": false
}
```

### HTTP Request

`PATCH https://apigw-dev.trendeo.com/user/address/{add_address_id_here}`

### Query Parameters

Parameter | Required? | Description
--------- | ------- | -----------
firstName | yes | First name
lastName | yes | Last name
email | yes | Email address
market | yes | Must be one of our active market IDs
street1 | yes | Address field 1
street2 | no | Address field 2
street3 | no | Address field 3
city | yes | City
postalCode | yes | Zip code; no validation done in backend
phone | yes | Phone number; no validation done in backend (e.g. allow numerical only or whatever)
newsletterAllowed | no | Is the user fine with being subscribed? Either true or false, false is default. The API does NOT subscribe the user rightaway, this only happens when an order is placed.
primary | no | Is this the primary address of the user? True or false, false is default
primaryDelivery | no | Is this the primary delivery address of the user? True or false, false is default
primaryBilling | no | Is this the primary billing address of the user? True or false, false is default

### Response

The endpoint updates the address with the given address ID of the user with the provided JWT.
IMPORTANT: In case there is no address with the given ID, the address is created with exactly that ID and returned.

Parameter | Description
--------- | -------
id | Newly created alphanumerical ID of the new address
firstName | First name
lastName | Last name
companyName | Deprecated field, should be always null
street1 | Address field 1
street2 | Address field 2
street3 | Address field 3
city | City
postalCode | Zip code
state | Deprecated field, should be always null
newsletter | Is the user fine with being subscribed with the email address of that address?
market | Market which the address is assigned to
phone | Phone number
email | Email address
addressType | Deprecated field, should always be "RESIDENTIAL"
primary | Is this the primary address of the user?
primaryDelivery | Is this the primary delivery address of the user?
primaryBilling | Is this the primary billing address of the user?

## Delete an address

> To delete an address, use this code:

```shell
curl -X DELETE "https://apigw-dev.trendeo.com/user/address/{add_address_id_here}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc"
```

> The above command returns just the code 200 without body.

### HTTP Request

`DELETE https://apigw-dev.trendeo.com/user/address/{add_address_id_here}`

### Query Parameters

Just the address ID in the URL.

### Response

Address gets deleted, only response code 200 is returned without any body.
IMPORTANT: In case an address is deleted which has any "primary" flag, no other of the remaining addresses is marked primary, i.e. there will be no primary in that case. If any client has a different expectation, let's discuss - for now it works like that.
In case there is no address with the given address ID, a 404 error is returned.

## Get specific address

> To get one specific address, execute the following:

```shell
curl -X GET "https://apigw-dev.trendeo.com/user/address/{add_address_id_here}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc"
```

> The above command returns the requested address as JSON structure:

```json
{
    "id": "JQmPj7NG9",
    "firstName": "Dominik",
    "lastName": "Picker",
    "companyName": null,
    "street1": "Obernstraße 39",
    "street2": "Appartment 11",
    "street3": "Left entrance",
    "city": "London",
    "postalCode": "N1 6DR",
    "state": null,
    "newsletter": true,
    "country": "UK",
    "phone": "+7 915 4334358",
    "email": "dominik@trendeo.com",
    "addressType": "RESIDENTIAL",
    "primary": false,
    "primaryDelivery": true,
    "primaryBilling": false
}
```

### HTTP Request

`GET https://apigw-dev.trendeo.com/user/address/{add_address_id_here}`

### Query Parameters

None, except address ID in URL.

### Response

The endpoint returns the requested address as JSON structure, or 404 in case it does not exist for the given user.

Parameter | Description
--------- | -------
id | Newly created alphanumerical ID of the new address
firstName | First name
lastName | Last name
companyName | Deprecated field, should be always null
street1 | Address field 1
street2 | Address field 2
street3 | Address field 3
city | City
postalCode | Zip code
state | Deprecated field, should be always null
newsletter | Is the user fine with being subscribed with the email address of that address?
market | Market which the address is assigned to
phone | Phone number
email | Email address
addressType | Deprecated field, should always be "RESIDENTIAL"
primary | Is this the primary address of the user?
primaryDelivery | Is this the primary delivery address of the user?
primaryBilling | Is this the primary billing address of the user?

## Get all addresses

> To get all addresses of a user, execute the following:

```shell
curl -X GET "https://apigw-dev.trendeo.com/user/address" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc"
```

> The above command returns the requested address as JSON structure:

```json
{
    "58ek9nd3K": {
        "id": "58ek9nd3K",
        "firstName": "Dominik",
        "lastName": "Picker",
        "companyName": null,
        "street1": "Obernstraße 39",
        "street2": null,
        "street3": null,
        "city": "London",
        "postalCode": "N1 6DR",
        "state": null,
        "newsletter": true,
        "country": "UK",
        "phone": "+7 915 4334358",
        "email": "dominik@trendeo.com",
        "addressType": "RESIDENTIAL",
        "primary": false,
        "primaryDelivery": false,
        "primaryBilling": false
    },
    "dnJgQllne": {
        "id": "dnJgQllne",
        "firstName": "Dominik",
        "lastName": "Picker",
        "companyName": null,
        "street1": "Obernstraße 39",
        "street2": "Appartment 11",
        "street3": "Left entrance",
        "city": "London",
        "postalCode": "N1 6DR",
        "state": null,
        "newsletter": true,
        "country": "UK",
        "phone": "+7 915 4334358",
        "email": "dominik@trendeo.com",
        "addressType": "RESIDENTIAL",
        "primary": false,
        "primaryDelivery": false,
        "primaryBilling": false
    }
}
```

### HTTP Request

`GET https://apigw-dev.trendeo.com/user/address`

### Query Parameters

None.

### Response

The endpoint returns all addresses of the given user, as a dictionary with address IDs as keys. An empty object is returned in case there is no address yet.
Each address has the already well-known structure:

Parameter | Description
--------- | -------
id | Newly created alphanumerical ID of the new address
firstName | First name
lastName | Last name
companyName | Deprecated field, should be always null
street1 | Address field 1
street2 | Address field 2
street3 | Address field 3
city | City
postalCode | Zip code
state | Deprecated field, should be always null
newsletter | Is the user fine with being subscribed with the email address of that address?
market | Market which the address is assigned to
phone | Phone number
email | Email address
addressType | Deprecated field, should always be "RESIDENTIAL"
primary | Is this the primary address of the user?
primaryDelivery | Is this the primary delivery address of the user?
primaryBilling | Is this the primary billing address of the user?
