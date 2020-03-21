# [02 - Accessing Data in an OAuth Server](https://www.oauth.com/oauth2-servers/accessing-data/)
Example application using the GitHub API, listing a logged-in users repositories

## [2.1 - Create an Application](https://www.oauth.com/oauth2-servers/accessing-data/create-an-application/)
- [Create OAuth application](https://github.com/settings/developers) in your GitHub account
- Configure redirect url
- Receive `client_id` and `client_secret`

## [2.2 - Setting up the Environment](https://www.oauth.com/oauth2-servers/accessing-data/setting-up-the-environment/)
- PHP application
- Use session to store state between redirects

## [2.3 - Authorization Request](https://www.oauth.com/oauth2-servers/accessing-data/authorization-request/)
- Request user authorization for your application to the GitHub API
- Redirect to GitHub authorization url
    - url: `https://github.com/login/oauth/authorize`
    - query parameters:
        - `response_type`: `code`
        - `client_id`: `<app client_id>`
        - `redirect_uri`: uri to redirect to after user authorization
        - `scope`: space separated list of scopes the application request user authorization for
        - `state`: randomly generated value, stored in session
        
## [2.4 - Obtaining an Access Token](https://www.oauth.com/oauth2-servers/accessing-data/obtaining-an-access-token/)
- Redirect to application redirect url
    - url: `redirect_uri`
    - query parameters:
        - `state`: should be same as in initial authorization request to make sure request was initiated by the application
        - `code`: code which can be exchanged for an access token
- Exchange code for access token
    - url: `https://github.com/login/oauth/access_token`
    - query parameters:
        - `grant_type`: `authorization_code`
        - `client_id`: `<app client_id>`
        - `client_secret`: `<app client_secret>`
        - `redirect_uri`: optional uri to redirect to after token exchange
        - `code`: code from previous redirect 
- Access token is returned and saved on the session
 
## [2.5 - Making API Requests](https://www.oauth.com/oauth2-servers/accessing-data/making-api-requests/)
- Add authorization header to API requests: `Authorization: Bearer <access_token>` 