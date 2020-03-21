# [03 - Signing in with Google](https://www.oauth.com/oauth2-servers/signing-in-with-google/)
- OAuth is often used as authentication protocol, identify a user logging in to a 3rd party app
- OAuth is an authorization protocol, end result is an access token
- Service provides 'user info' endpoint where access token can be used to get profile information
- More advanced and standardized approach: OpenId Connect

## [3.1 - Create an Application](https://www.oauth.com/oauth2-servers/signing-in-with-google/create-an-application/)
- [Create project](https://console.developers.google.com/) in your Google account
- Create OAuth 2.0 credentials
    - Name, url, logo, ...
    - Configure redirect url
    - Receive `client_id` and `client_secret`

## [3.2 - Setting up the Environment](https://www.oauth.com/oauth2-servers/signing-in-with-google/setting-up-the-environment/)
- PHP application
- Use session to store state between redirects

## [3.3 - Authorization Request](https://www.oauth.com/oauth2-servers/signing-in-with-google/authorization-request/)
- Request user authentication for your application using the Google API
- Redirect to GitHub authorization url
    - url: `https://accounts.google.com/o/oauth2/v2/auth`
    - query parameters:
        - `response_type`: `code`
        - `client_id`: `<app client_id>`
        - `redirect_uri`: uri to redirect to after user authorization
        - `scope`: `openid email` (OpenId Connect scopes, not requesting access to the user's data, just for identification)
        - `state`: randomly generated value, stored in session
- Redirect screen is not a typical OAuth screen, because user isn’t granting any permissions, just identifying himself

## [3.4 - Getting an ID Token](https://www.oauth.com/oauth2-servers/signing-in-with-google/getting-an-id-token/)
- Redirect to application redirect url
    - url: `redirect_uri`
    - query parameters:
        - `state`: should be same as in initial authorization request to make sure request was initiated by the application
        - `code`: code which can be exchanged for an id token
- Exchange code for id token
    - url: `https://www.googleapis.com/oauth2/v4/token`
    - query parameters:
        - `grant_type`: `authorization_code`
        - `client_id`: `<app client_id>`
        - `client_secret`: `<app client_secret>`
        - `redirect_uri`: optional uri to redirect to after token exchange
        - `code`: code from previous redirect 
    - response
        - access token
        - JWT id token

## [3.5 - Verifying the User Info](https://www.oauth.com/oauth2-servers/signing-in-with-google/verifying-the-user-info/)
1. Decode JWT to retrieve user information
2. Use id token to retrieve user information
    - `https://www.googleapis.com/oauth2/v3/tokeninfo`
    - found in [OpenId Connect Discovery Document](https://accounts.google.com/.well-known/openid-configuration)
3. Use access token to retrieve user information
    - part of the OpenID Connect standard 
    - `https://www.googleapis.com/oauth2/v3/userinfo`
    - found in [OpenId Connect Discovery Document](https://accounts.google.com/.well-known/openid-configuration)    
