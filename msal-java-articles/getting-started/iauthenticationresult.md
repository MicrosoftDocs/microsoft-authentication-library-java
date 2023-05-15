In all of the authentication flows, the methods to acquire tokens return a `Future<IAuthenticationResult>`.
The AuthenticationResult interface contains the following attributes:

* `accessToken` for the Web API access. This is a string, usually a base64 encoded JWT but the client should never look inside the access token. The format isn't guaranteed to remain stable and it can be encrypted for the resource. People writing code depending on access token content on the client is one of the biggest sources of errors when client logic breaks.
* `idToken` a token representing the authentication of the user (this is a JWT)
* `expiresOnDate` tells the date/time when the token expires
* `account`
* `environment`
* `scopes` set of permissions which were requested

