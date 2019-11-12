# Servicechains and Contacts

Now that a User and an `Organization` exist, there are only a couple more requirements for being able to send a message - a recipient, and a way of sending it; or, in Voluble's terms, a `Contact` and a `Servicechain`.

## Creating a Servicechain

A `Servicechain` is a list of `Services` that Voluble will use in order to try and send a `Message` to a `Contact`. We have to create a `Servicechain` in order to tell Voluble how to send our `Messages`, as there may be several `Services` available, and no way for `Voluble` to decide automatically which one to use.

### Querying the Services

The first thing to do before we can create a `Servicechain` is see which `Services` are actually available. To do this, query the `/services` endpoint:

{% api-method method="get" host="https://voluble-poc.herokuapp.com/v1" path="/services" %}
{% api-method-summary %}
GET /services
{% endapi-method-summary %}

{% api-method-description %}
Retrieve a list of the Services available to Voluble.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
The access token representing the User, supplied as a Bearer token
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of the Services available in Voluble is provided.
{% endapi-method-response-example-description %}

```javascript
{
    "status": "success",
    "data": {
        [
            {
                "id": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
                "name": "Telegraph",
                "directory_name": "telegraph"
            },
            {
                "id": "c5bf2fb1-b8ec-424e-a378-db46e2d90cab",
                "name": "WhatsUp",
                "directory_name": "whatsup"
            }
        ]
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Now that we have a list of the `Services` available \(in this example, 'Telegraph and 'WhatsUp',\) we can decide which order we want Voluble to use them in, in order to send a `Message` to the `Contact` we're about to create.

This means we're creating a `Servicechain`. Let's say we want Voluble to try sending a `Message` via 'WhatsUp', and if that fails, then try 'Telegraph'. We'll call this `Servicechain` 'Basic Online Servicechain', since it only uses online `Services`, rather than SMS.

This means our `Servicechain` looks like this:

```javascript
{
    "name": "Basic Online Servicechain",
    "services":
        [
            {
                "id": "c5bf2fb1-b8ec-424e-a378-db46e2d90cab", // The ID of the WhatsUp Service
                "priority": 1 // Use this Service first
            },
            {
                "id": "c616d114-4f68-4dba-b976-cd5ae0aa359d", // The ID of the Telegraph Service
                "priority": 2
            }
        ]
}
```

This is what we'll submit to the `/servicechains` endpoint, as follows:

{% api-method method="post" host="https://voluble-poc.herokuapp.com/v1" path="/servicechains" %}
{% api-method-summary %}
POST /servicechains
{% endapi-method-summary %}

{% api-method-description %}
Create a new Servicechain with the specified Services
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
The access token representing the User, supplied as a Bearer token
{% endapi-method-parameter %}

{% api-method-parameter name="Content-Type" type="string" required=true %}
**MUST** be 'application/json'
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="services" type="array" required=true %}
An array of Objects containing the ID of the Service to add \('id'\) and the priority in which to add it \('priority'\)
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
The name of the new Servicechain
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
The Servicechain has successfully been created.
{% endapi-method-response-example-description %}

```javascript
{
  "id": "893ccd5f-d4e8-4fb4-a932-054e41d1fd08",
  "services": [
    {
      "id": "c616d114-4f68-4dba-b976-cd5ae0aa639c",
      "service": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
      "priority": 1
    },
    {
      "id": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
      "service": "c5bf2fb1-b8ec-424e-a378-db46e2d90cab",
      "priority": 2
    }
  ],
  "name": "Basic Online Servicechain"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Creating a Contact

To create a Contact, use the `POST` method on the `/contacts` endpoint. The `Organization` that the `Contact` will be added to is automatically deduced from the User's ID in the `sub` clause in the JWT provided as the access token.

We can provide the ID of the `Servicechain` we just created, and that will become the default`Servicechain` for the `Contact` when sending a `Message`.

{% api-method method="post" host="https://voluble-poc.herokuapp.com/v1" path="/contacts" %}
{% api-method-summary %}
POST /contacts
{% endapi-method-summary %}

{% api-method-description %}
Create a new Contact with the specified details, and add it to the Organization represented by the User specified in the access token.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
The access token representing the User, supplied as a Bearer token
{% endapi-method-parameter %}

{% api-method-parameter name="Content-Type" type="string" required=true %}
**MUST** be 'application/json'
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="Servicechain\_ID" type="string" required=true %}
The ID of the default Servicechain that Voluble will use to send a Message to this Contact
{% endapi-method-parameter %}

{% api-method-parameter name="email\_address" type="string" required=true %}
The email address for the Contact
{% endapi-method-parameter %}

{% api-method-parameter name="phone\_number" type="string" required=true %}
The phone number of the Contact, to which Messages will be sent. Should be E164-compliant
{% endapi-method-parameter %}

{% api-method-parameter name="surname" type="string" required=true %}
Surname of the Contact to create
{% endapi-method-parameter %}

{% api-method-parameter name="first\_name" type="string" required=true %}
The first name of the Contact to create
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
The Contact has successfully been created.
{% endapi-method-response-example-description %}

```javascript
{
  "status": "success",
  "data": {
    "id": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
    "first_name": "John",
    "surname": "Cleese",
    "email_address": "myname@emailprovider.com",
    "phone_number": "+442071838750",
    "OrganizationId": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
    "ServicechainId": "893ccd5f-d4e8-4fb4-a932-054e41d1fd08"
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

