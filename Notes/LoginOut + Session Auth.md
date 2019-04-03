# Login/Out + Session Auth

-   Rather than having to manage user authentication and the pain that goes with password management, we hand user management off to Auth0.
-   In order for the Voluble web app to make requests to Voluble on behalf of the user, we use the Auth0 [Authorization Code Grant](https://auth0.com/docs/api-auth/tutorials/authorization-code-grant). ([More info](https://auth0.com/docs/api-auth/grant/authorization-code))
-   So, the flow looks like this:

```sequence
Title: AuthenticationFlow
participant VolubleWebsite
participant VolubleApi
participant Auth0
Note over VolubleWebsite,Auth0: User wants to log in to the webapp
VolubleWebsite->Auth0: Redirect user to Auth0 login page
Auth0-->Auth0: Authenticate the user
Auth0->VolubleWebsite: Redirect the user back to the Voluble webapp with an Auth Code
VolubleWebsite->Auth0: Request an Access Token using the Auth Code
Auth0-->Auth0: Authenticate the webapp
Auth0->VolubleWebsite: Returns an Access Token with the user's scopes
Note over VolubleWebsite,Auth0: User wants to send a message
VolubleWebsite->VolubleApi: POST /messages with Access Token
VolubleApi-->VolubleApi:Authenticate access token
VolubleApi-->VolubleApi:Init message-sending process
VolubleApi->VolubleWebsite: 200 OK with message data
```



