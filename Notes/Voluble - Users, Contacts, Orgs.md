# Voluble - Users, Contacts, Orgs

-   Difference between users and contacts?
    -   Users can log into the system, contacts *can't*.
    -   A contact is just a name, email, phone and contact info
    -   A user has **privileges** - different privileges enable different operations to be carried out by the user.
    -   Is a user just a souped-up contact?
        -   Arguably, no - a contact _cannot_ log in
        -   When trying logins, we don't want to search through a vast `contacts` table, just a smaller `users` table.
    -   But, how to avoid duplication of contact info between users and contacts (assuming every user has a corresponding contact?)
        -   Can DB associations do this for us?
        -   This may not actually be necessary - a user doesn't have any contact info associated with it, only a reference to a contact, and the privileges it holds. **TODO: Make sure this is true with Auth0.**
-   In short:
    -   Users: stored by Auth0 (so we don't need to handle password management and all of the shit that goes with it)
    -   Contacts: stored by Voluble (albeit in a third-party database, over HTTPS)

## Users

-   Users mainly exist on Auth0, all we do is keep their Auth0 ID
    -   How do we speed up the login process?
    -   Or do we hand the entire process off to Auth0?
        -   Most likely, yes
-   Users (as defined in Auth0) can have a number of **privileges** (defined in Auth0 as _scopes_ - these could include:

| Functional Privileges | Access Privileges |
| --------------------- | ----------------- |
| `createContact`       | `thisOrg`         |
| `updateContact`       | `allOrgs`         |
| `deleteContact`       |                   |
| `sendMessage`         |                   |

### What does Voluble need to know about users?

-   ~~Scopes~~
    -   Actually, no - this is all handled by Auth0 as part of the `access_token`
-   Organization ID
    -   Can this be built into the `access_token`?
        -   This would pretty much remove any necessity for Voluble to communicate with Auth0 for the sake of Authentication/Access control
        -   Could be built into a middleware
        -   Just requires Voluble to keep Auth0 honest when changing the `User`'s `Organization`
    -   That said, the `access_token` contains the `User'`s Auth0 ID in the `sub` claim - we could always use this to confirm the `User`'s `Organization` by looking up their Auth0 ID.
-   Auth0 ID
    -   This enables `Organization` owners to be able to see a list of users in the `Organization`
-   Regarding access control, everything else should be in the `access_token` JWT
-   However, we do need to have an endpoint to get/set metadata for updating scopes
    -   All this would do is effectively be a wrapper around the Auth0 endpoint

## Orgs

-   An `Organization` is a collection of users, in essence.
-   A user cannot own (or potentially, have access to? what about Voluble admins? Does the `allOrgs` privilege come into place?) more than one `Organization`
-   It's fair to say that an `Organization` is the main boundary and delimiter within Voluble. As soon as a new user signs up, they _cannot do anything_ without _joining_ or _starting_ an `Organization`.