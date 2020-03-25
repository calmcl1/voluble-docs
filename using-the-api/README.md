# Using the API

Voluble exposes a [REST API](https://app.swaggerhub.com/apis/voluble/voluble-api/1.0) in order to send `Messages`, to create and manipulate `Contacts`,`Organizations`, and `Servicechains`, and to view `Services`.

## Authentication

The majority of methods behind the `/v1` API require authentication. This requires a [JSON Web Token](https://jwt.io) passed in the `Authorization` header, using the `Bearer` method, as such:

```text
curl -H "Authorization: Bearer ${TOKEN}" https://voluble-poc.herokuapp.com/v1/messages
```

The JWT contains all of the necessary information about the scopes that the user has - and as such, which functions are available to the user.

Voluble uses [Auth0](https://auth0.com) as the identity manager for Users. As such, a valid JWT can be obtained by creating a login with Auth0 for the `voluble-dev.eu.auth0.com` tenant. For more details on signup and retrieving a token from Auth0, see [their documentation](https://auth0.com/docs/api/authentication?javascript#signup).

### Scopes

The scopes in a JWT control the routes and methods that are available on a User-by-User basis, which allows an `Organization` to have different tiers of Users, should they wish.

#### Contact Scopes

| Scope Name | Privilege |
| :--- | :--- |
| `contact:add` | Allows the User to add new `Contacts`. |
| `contact:view` | Allows the User to view the existing `Contacts`. All Users should have this if they also have `message:send`. |
| `contact:edit` | Allows the User to modify existing `Contacts`. |
| `contact:delete` | Allows the User to remove existing `Contacts`. |

#### Message Scopes

| Scope Name | Privilege |
| :--- | :--- |
| `message:read` | Allows the User to read `Messages` that currently exist on Voluble. |
| `message:send` | Allows the User to send new `Messages` to existing `Contacts`. |
| `blast:send` | Allows the User to send new `Blasts` \(i.e. send the same `Message` to many `Contacts`. |

#### Organization and User Scopes

| Scope Name | Privilege |
| :--- | :--- |
| `user:view` | Allows the User to view the existing Users in the `Organization`. |
| `user:add` | Allows the User to add new Users to the `Organization`. |
| `user:edit` | Allows the User to modify existing Users in the `Organization`. |
| `user:delete` | Allows the User to remove existing Users from the `Organization`. |
| `organization:edit` | Allows the User to edit detail about the `Organization` \(e.g. phone number,  name, etc.\) |
| `organization:delete` | Allows the User to delete the `Organization`, its `Messages` and `Contacts` as a whole. Should be reserved for high-level Users only. |
| `organization:owner` | Implicitly allows `organization:edit` and `organization:delete`. Automatically awarded to the first User in the `Organization`. |

#### Service and Servicechain Scopes

| Scope Name | Privilege |
| :--- | :--- |
| service:view | Allows the User to view the existing `Services` in Voluble. |
| servicechain:view | Allows the User to view the existing `Servicechains` in the `Organization`. All Users should have this if they also have `message:send`. |
| servicechain:add | Allows the User to create a new `Servicechain` for the `Organization`. |
| servicechain:delete | Allows the User to remove existing `Servicechains` from the `Organization`. |
| servicechain:edit | Allows the User to modify existing `Servicechains` in the `Organization`. |

#### Credit and Billing Scopes

| Scope Name | Privilege |
| :--- | :--- |
| credits:update | Allows the User to modify the amount of `credits` that the `Organization` has. Should be controlled with a billing system on the implementation end. |

