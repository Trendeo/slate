# Errors

All API endpoints answer with the same error structure.
Exception: In case an error happens on API-Gateway or Traefik-loadbalancer-level, the client should look only on the response code.

Generally, an error message looks like the following example:

```json
{
  "status": "BAD_REQUEST",
  "timestamp" : "2018-02-20 13:31:48",
  "message": "The provided request is incomplete",
  "debugMessage": "One or more fields are missing or have not passed validation; please check the list of subErrors for details",
  "subErrors": {
    "object": "user",
    "field": "firstName",
    "rejectedValue": "",
    "message": "The firstName field may not be empty"
  }
}
```

Important note: The structure of the subError objects may differ from error to error. Only the general error structur is guaranteed to stay stable. Please process the subErrors with strict structure programmatically only when strictly required; otherwise use for debugging / testing purposes only.
The HTTP response code corresponds to the string provided in the "status" field.

Besides endpoint-specific errors, following errors can be expected from all endpoints:


Error Code | Meaning
---------- | -------
400 | Bad Request -- The request is invalid because of incorrect JSON-structure, missing fields, or fields not passing semantic validation. This error is also thrown in case of specific errors during processing, e.g. when you try to sign up a user that already exists.
401 | Unauthorized -- You better get yourself a JWT.
403 | Forbidden -- The provided JWT is expired or has invalid content. Also, this error is thrown in case a refreshToken or similar is invalid or expired. Furthermore, in case of login with invalid credentials.
404 | Not Found -- Either the specific API endpoint does not exist, or the resource you try to access does not exist (e.g. logging in a user that does not exist).
405 | Method Not Allowed -- The HTTP request method you used is not allowed for that endpoint.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The kitten requested has been removed from our servers.
418 | I'm a teapot.
429 | Too Many Requests -- Somebody wrote bad code and ended up in an infinite loop
500 | Internal Server Error -- Everything is broken and we are doomed.
503 | Service Unavailable -- Everything is even more broken and you better run.
