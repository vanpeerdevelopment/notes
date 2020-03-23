# [17 - Protecting Mobile Apps with PKCE](https://www.oauth.com/oauth2-servers/pkce/) 
- Proof Key for Code Exchange (PKCE, pronounced pixie)
- Technique for public clients to mitigate the threat of having the authorization code intercepted
- Client creates a secret sent in authorization request, and then uses secret again when exchanging the code for an access token

## [17.1 - Authorization Request](https://www.oauth.com/oauth2-servers/pkce/authorization-request/)
- App creates a `code_verifier`: random string between 43 and 128 characters
- Use `code_verifier` to create `code_challenge`: `base64(sha256(code_verifier))` 
- Send authorization request with extra query parameters
    - `code_challenge`=`<code_challenge>`
    - `code_challenge_method`=`S256 | plain`
- Service saves the `code_challenge` with the `code` it generates and sends in redirect
- Service can enforce public clients to use PKCE

## [17.2 - Authorization Code Exchange](https://www.oauth.com/oauth2-servers/pkce/authorization-code-exchange/)
- Send code exchange request with extra query parameter
    - `code_verifier`=`<code_verifier>`
- Service calculates `code_challenge` for received `code_verifier` and validates it is equal to initial `code_challenge`