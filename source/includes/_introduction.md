# Introduction

Welcome to the reference documentation of the backend APIs for trendeo.com! This documentation should provide all details you need to work with the backend. In case anything doesn't work as you expect or you have some suggestions for improvement, please shoot to dominik@trendeo.com!

# API Hosts

There are two different API hosts for all backend APIs - one for staging and one for production. **https://apigw.trendeo.com** works for production, and **https://apigw-dev.trendeo.com** for staging - choose wisely ;)
All API endpoints below are referring to the staging environment. Please always use HTTPS, HTTP might get deprecated at some point in the future.

# Passing parameters

All POST endpoints expect JSON as body. Please always include "application/json" in the Content-type header.

All GET endpoints expect parameters as regular query-string parameters.
