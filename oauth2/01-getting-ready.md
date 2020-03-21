# [01 - Getting Ready](https://www.oauth.com/oauth2-servers/getting-ready/)
    
## Application registration
1. Sign up (as developer) with the OAuth 2.0 service 
    - e.g. dietervp@hotmail.com on Strava
2. Create application as developer
    - name
    - description
    - website 
    - logo
    - redirect url(s)
    - ... 
    - e.g. Kunlabora Strava Dashboard
3. Receive `client_id` and `client_secret` for the application

## Redirect URL(s)
- Allowed URL(s) where service will redirect to when a user authorized the application
- `https` required: prevent code from being intercepted
- Avoid query parameter, just include a path

## State
- Created by application and passed to service during initial authorization request
- e.g. JWT containing redirect url for UI and some random data
- Returned in redirect by service when user authorizes application
- Used to encode application state: take user back to correct location in UI after sign in
- Include random data: make sure OAuth redirect was triggered by application