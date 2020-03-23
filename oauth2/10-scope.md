# [10 - Scope](https://www.oauth.com/oauth2-servers/scope/) 
Scope is a way to control access and help the user identify the permissions they are granting to the application.

## [10.1 - Defining Scopes](https://www.oauth.com/oauth2-servers/scope/defining-scopes/)
- Not too many, understandable scopes
- Read vs. write
- Restricting access to sensitive info, e.g. private GitHub repos
- Enable access to a user’s account based on functionality, e.g. drive, gmail, youtube, ...
- Limiting access to billable resources

## [10.2 - User Interface](https://www.oauth.com/oauth2-servers/scope/user-interface/)
- Display requested scopes in a human-readable way
- Show scopes which are not requested
- If request grants full access to a user’s account, the service should make it abundantly clear

## [10.3 - Checkboxes](https://www.oauth.com/oauth2-servers/scope/checkboxes/)
- Grant an access token with less scope than the application requests
- Send granted scopes in access token response 