# [06 - Mobile and Native Apps](https://www.oauth.com/oauth2-servers/mobile-and-native-apps/)
- Can not keep `client_secret` confidential
- Authorization code is exchanged for an access token without using the `client_secret`
- Use Google's open source library AppAuth

## [6.1 - Authorization](https://www.oauth.com/oauth2-servers/mobile-and-native-apps/authorization/)
1. Application starts authorization request in native browser or `SFSafariViewController`
    - `response_type`: `code`
    - `redirect_uri`
        - iOS: custom url scheme such as `org.example.app://`
        - android: install url matching pattern for app
2. User authorizes application
3. Redirect to application with `code` and `state` query parameters
4. `code` exchanged for access token using `client_id` and without `client_secret`

## [6.2 - Security Considerations](https://www.oauth.com/oauth2-servers/mobile-and-native-apps/security-considerations/)
- Open native browser or use `SFSafariViewController`
- Use Proof Key for Code Exchange (PKCE, pronounced pixie)