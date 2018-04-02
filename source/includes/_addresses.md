# User addresses

Users can have as many addresses as they like. Every address receives an alphanumerical ID at time of creation. Addresses can be "primary" in general, and primary for either delivery or billing (or both). All address API endpoints require a valid JWT, i.e. are access restricted.

Some more remarks about the "primary" flags: The API ensures that at any given time at least one address per market is primary, primary delivery or primary billing. Of course these addresses might be different ones, but it is guaranteed that you will find at least one address per market with one or multiple of these three flags.
In case an operation is performed which breaks this guarantee (e.g. market of primary address is changed, primary address is deleted, primary fields are updated to false), the API randomly assigns another address as primary to comply.

However, the API does NOT guarantee that there is EXACTLY one primary address per type. It might happen that there will be more than one, so make sure your client application can deal with this case!

## Create an address

> To create an address, use this code:

```shell
curl -X POST "https://apigw-dev.trendeo.com/user-restricted/user/address" \
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

`POST https://apigw-dev.trendeo.com/user-restricted/user/address`

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

A successful request creates the address with a newly created address ID. In case any of the "primary" fields is set to "true", any other address having that field set to "true" is turned into "false" for the market of the new address, to make sure that there is just one primary address at any point in time.
In case the address is created for a guest user, and this guest user does not have a database entry yet, this guest user will be created in database.

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
curl -X PATCH "https://apigw-dev.trendeo.com/user-restricted/user/address/{add_address_id_here}" \
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

`PATCH https://apigw-dev.trendeo.com/user-restricted/user/address/{add_address_id_here}`

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
In case this change violates the guarantee that there is at least one address with for each primary-flag, the API assigns addresses as primary randomly to fulfill the promise. Furthermore, other primary addresses become non-primary if the updated address is primary.
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
curl -X DELETE "https://apigw-dev.trendeo.com/user-restricted/user/address/{add_address_id_here}" \
  -H "Content-type: application/json" \
  -H "Authorization: Bearer abc"
```

> The above command returns just the code 200 without body.

### HTTP Request

`DELETE https://apigw-dev.trendeo.com/user-restricted/user/address/{add_address_id_here}`

### Query Parameters

Just the address ID in the URL.

### Response

Address gets deleted, only response code 200 is returned without any body.
In case the deleted address was primary, another address in that market is made primary randomly.
IMPORTANT: In case an address is deleted which has any "primary" flag, no other of the remaining addresses is marked primary, i.e. there will be no primary in that case. If any client has a different expectation, let's discuss - for now it works like that.
In case there is no address with the given address ID, a 404 error is returned.

## Get specific address

> To get one specific address, execute the following:

```shell
curl -X GET "https://apigw-dev.trendeo.com/user-restricted/user/address/{add_address_id_here}" \
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

`GET https://apigw-dev.trendeo.com/user-restricted/user/address/{add_address_id_here}`

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
curl -X GET "https://apigw-dev.trendeo.com/user-restricted/user/address" \
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

`GET https://apigw-dev.trendeo.com/user-restricted/user/address`

### Query Parameters

None.

### Response

The endpoint returns all addresses of the given user, as a dictionary with address IDs as keys. An empty object is returned in case there is no address yet. In case the addresses of a guest user are requested, but the guest user does not exist yet in the database, just an empty list is returned.
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


## Get all addresses for a specific market

> To get all addresses for one market of a user, execute the following:

```shell
curl -X GET "https://apigw-dev.trendeo.com/user-restricted/user/address/market/{market}" \
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

`GET https://apigw-dev.trendeo.com/user-restricted/user/address/market/{market}`

### Query Parameters

Market ID in path.

### Response
The same structure like in the market-unspecific endpoint (see above)
