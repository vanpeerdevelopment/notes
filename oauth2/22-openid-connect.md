# [22 - OpenID Connect](https://www.oauth.com/oauth2-servers/openid-connect/) 
- OAuth 2.0 is a delegation framework, allowing apps to act on behalf of a user, without needing to know the identity of the user
- OpenID Connect takes the OAuth 2.0 framework and adds an identity layer on top

## [22.1 - Authorization vs Authentication](https://www.oauth.com/oauth2-servers/openid-connect/authorization-vs-authentication/)
- OAuth 2.0 access token says that a user delegated an application to act on their behalf
- OAuth 2.0 access token does not indicate who a user is, you can just use it to access data during a certain period

## [22.2 - Building an Authentication Framework](https://www.oauth.com/oauth2-servers/openid-connect/building-an-authentication-framework/)
- OAuth 2.0 as the basis of an authentication protocol
    - Define endpoint for user info
    - Define scope(s) to request user info
    - Define error codes and extension parameters (e.g. reprompt login)
- OpenID connect: authentication standard

## [22.3 - ID Tokens](https://www.oauth.com/oauth2-servers/openid-connect/id-tokens/)
- JWT encoding the user’s authentication information, sent besides access token
- Signed with the private key of the issuer
- Can be parsed and verified by the application
- JWT payload
    - `sub`: subject: user id
    - `iss`: issuer: token issuer id
    - `aud`: audience: client application id
    - `exp`: expiration date 

## [22.4 - Summary](https://www.oauth.com/oauth2-servers/openid-connect/summary/)
- [Documentation](https://openid.net/connect/)
- [OpenID Connect debugger](https://oidcdebugger.com/)