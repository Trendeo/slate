# Introduction

Welcome to the reference documentation of the backend APIs for trendeo.com! This documentation should provide all details you need to work with the backend. In case anything doesn't work as you expect or you have some suggestions for improvement, please shoot to dominik@trendeo.com!

# API Hosts & paths

There are two different API hosts for all backend APIs - one for staging and one for production. **https://apigw.trendeo.com** works for production, and **https://apigw-dev.trendeo.com** for staging - choose wisely ;)
All API endpoints below are referring to the staging environment. Please always use HTTPS, HTTP might get deprecated at some point in the future.
Generally, all endpoints are clearly separated into endpoints that require a JWT / authentication, and endpoints which are public and open to use without authentication. (Almost) All endpoints which require a JWT have "/user-restricted" in the begin of their paths. Open API endpoints do not begin with this pattern. This separation makes it (a) easier to manage the API configuration at the API-Gateway, and (2) makes it immediately obvious which endpoint is access restricted and which not.
We might add other prefixes like e.g. "/merchant-restricted" for endpoints for the merchant admin at a later stage. For now all merchant endpoints (e.g. price actions) are not covered by this rule.

# Passing parameters

All POST endpoints expect JSON as body. Please always include "application/json" in the Content-type header.

All GET endpoints expect parameters as regular query-string parameters.
