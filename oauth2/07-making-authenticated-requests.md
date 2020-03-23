# [07 - Making Authenticated Requests](https://www.oauth.com/oauth2-servers/making-authenticated-requests/)
Access token are opaque to the client, and should only be used to make API requests and not interpreted themselves

1. Header: `Authorization: Bearer <access_token>`
2. Post body parameter: `access_token=<access_token>`

## [7.1 - Refreshing an Access Token](https://www.oauth.com/oauth2-servers/making-authenticated-requests/refreshing-an-access-token/)
```
{
    "access_token": "AYjcyMzY3ZDhiNmJkNTY",
    "refresh_token": "RjY2NjM5NzA2OWJjuE7c",
    "token_type": "bearer",
    "expires": 3600
}
```

- `expires`: number of seconds `access_token` is valid
- Find out if access token is expired
    - store `expires` field
    - try to make request and check error
        - Status: `401`
        - Header: `WWW-Authenticate: Bearer error="invalid_token"`
        - Body: `{ "error": "invalid_token" }`
- Refresh access token using `refresh_token`
    - url: `<token_url>`
    - query parameters:
        - `grant_type`: `refresh_token`
        - `client_id`: `<app client_id>`
        - `client_secret`: `<app client_secret>`
        - `refresh_token`: `<refres_token>`
    - Response: new access and refresh token

