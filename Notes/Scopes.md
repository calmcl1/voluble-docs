# Scopes

Scopes as defined in Auth0 reflect the operations that certain users are allowed to undertake and the data that they are allowed to access. For example, a user with the scope `message:read` but not `message:send` will be able to read the messages of the `Organization` that they are a member of, but not send them.

## Potential Scope List

| Entity       | Scope                 | Capability                                                   |
| ------------ | --------------------- | ------------------------------------------------------------ |
| Contact      | `contact:add`         | Add a new Contact to the Organization                        |
|              | `contact:view`        | View a list of the Contacts in the Organization              |
|              | `contact:edit`        | Edit the Organization's Contacts                             |
|              | `contact:delete`      | Remove a Contact from the Organization                       |
| Message      | `message:read`        | See a list of all of the Organization's Messages             |
|              | `message:send`        | Send Messages on behalf of the Organization                  |
| Blast        | `blast:send`          | Send Blasts on behalf of the Organization                    |
| User         | `user:add`            | Add Users to the Organization                                |
|              | `user:edit`           | Edit the details of Users in the Organization                |
|              | `user:delete`         | Remove Users from the Organization                           |
|              | `user:view`           | View the Users in the Organization                           |
| Organization | `organization:edit`   | Edit the Organization's details                              |
|              | `organization:delete` | Delete the Organization, including it's Users, Contacts, and Messages |
|              | `organization:owner`  | Denotes a given User as being the owner of the Organization  |
| Service      | `service:view`        | See a list of the Services available in the Organization     |
| Servicechain | `servicechain:view`   | See a list of the Servicechains available in the Organization |
|              | `servicechain:add`    | Add a new Servicechain to the Organization                   |
|              | `servicechain:edit`   | Edit existing Servicechains in the Organization              |
|              | `servicechain:delete` | Remove existing Servicechains from the Organization          |
| Voluble      | `voluble:admin`       | This is reserved for internal use by Voluble sysadmins *only*, as it allows multi-Organization access. |

Note: There is no `service:add` scope as the services that are available is defined by Voluble's capability.