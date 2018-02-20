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
--------- | ------- | -----------
jwt | Your fresh and new JWT for future requests.
refreshToken | Token for getting a new JWT once its expired.

## Getting a JWT via Facebook login

## Refresh the JWT via your refresh token
