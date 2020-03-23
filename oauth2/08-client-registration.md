# [08 - Client Registration](https://www.oauth.com/oauth2-servers/client-registration/) 
Things to provide for client application registration when building an OAuth 2.0 service from scratch

## [8.1 - Registering a New Application](https://www.oauth.com/oauth2-servers/client-registration/registering-new-application/)
1. Create (developer) account
2. Create client application
    - name, icon, website, description, privacy policy, ...
    - redirect url(s)!
3. Receive `client_id` and `client_secret`

## [8.2 - The Client ID and Secret](https://www.oauth.com/oauth2-servers/client-registration/client-id-secret/)
- `client_id`: public id for app (often 32-character hex string)
- `client_secret`
    - Do not issue one for 'public' app (mobile, native, SPA): if it doesn't exist it can't be leaked
    - Issue one for 'web-server' app
    - shared secret between application and service
    - Use a cryptographically-secure library to generate a 256-bit value and convert it to a hexadecimal representation
    - Visually different from `client_id` to avoid accidentialy including it in public code (e.g. longer, secret prefix, ...)
    - Do not store `client_secret` plain text, store encrypted or hashed version

## [8.3 - Deleting Applications and Revoking Secrets](https://www.oauth.com/oauth2-servers/client-registration/deleting-applications-revoking-secrets/)
- Deleting applications
    - Inform about impact: access tokens will be revoked, number of users impacted
    - Revoke all access tokens, refresh tokens, code, ...
- Reset `client_secret`
    - Does not need to revoke all access tokens
    - Old `client_secret` immediately invalid