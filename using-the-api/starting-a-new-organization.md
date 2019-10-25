---
description: >-
  This is a quick guide to starting a new Organization with a new User from
  scratch
---

# Setting Up A New User

## Creating a new User

Before we can do anything, we need to create a new `User`. `Users` can own or work within `Organizations`, and every `Organization` has to have at least one `User` with the `organization:owner` [scope](./#scopes).

### Signing Up with Auth0

Since Voluble uses Auth0 for identity management, creation of Users must be handled by the Auth0 API, rather than by Voluble directly.

This can be done in a couple of ways - the [Auth0 documentation](https://auth0.com/docs/api/authentication?javascript#signup) is the best source of information on how to implement this within a front-end. Auth0 provides a [Javascript library](https://auth0.com/docs/libraries/auth0js/v9) in order to simplify most of the operations that a front-end might use.

{% hint style="info" %}
Note: the `domain` specified in the Auth0 documentation is `voluble-dev.eu.auth0.com`.
{% endhint %}

Once a new User has been created, an access token will be returned. This access token is a signed [JWT](https://jwt.io) representing the User, and this can be passed to Voluble. However, this User isn't part of an Organization, there isn't much it can do.

## Joining an Organization

### Creating a New Organization

Now, the User must create an Organization to be a part of. This represents a company, club, or group that may wish to send `Messages` to it's `Contacts`. To do this, send a `POST` request to Volubles' `orgs` endpoint.

{% api-method method="post" host="https://voluble-poc.herokuapp.com/v1" path="/orgs" %}
{% api-method-summary %}
POST /orgs
{% endapi-method-summary %}

{% api-method-description %}
Create a new Organization with the following parameters, and assign the current User to it.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Type" type="string" required=true %}
**MUST** be "application/json"
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
The access token representing the user, supplied as a Bearer token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="name" type="string" required=true %}
The name of the Organization
{% endapi-method-parameter %}

{% api-method-parameter name="phone\_number" type="string" required=true %}
The E164-compliant phone number that Voluble will use to send Messages on the Organization's behalf
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
The Organization has successfully been created.
{% endapi-method-response-example-description %}

```javascript
{
  "status": "success",

  "data": {
    "id": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
    "name": "YOUR ORGANIZATION NAME",
    "phone_number": "+442071838750" // Your Org's phone number
  }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
The supplied parameters \(either phone\_number or name\) are invalid.
{% endapi-method-response-example-description %}

```javascript
{
    "status": "fail"
    "data": {
        // The data supplied is dependent on the reason that the API call failed.
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

```text
$ curl -X POST \
 -H "Authorization: Bearer ${TOKEN} \
 -H "Content-Type: application/json"
 -d "{'name': 'My Corporation', 'phone_number': '+442071838750'}"
 
 >>> {"status": "success", "data": { "id": "c616d114-4f68-4dba-b976-cd5ae0aa359d", "name": "My Corporation", "phone_number":  "+442071838750" } }
```

If all has gone well, the User should now be part of a new Organization!

