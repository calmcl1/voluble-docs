# Sending a Message

Now that an [`Organization` has been created](setting-up-new-user.md#creating-a-new-user), and a [`Contact` has been set up with a `Servicechain`](servicechains-and-contacts.md#creating-a-servicechain), it's possible to send a message!

Simply use the endpoint `/messages` to send a new message.

{% api-method method="post" host="https://voluble-poc.herokuapp.com/v1" path="/messages" %}
{% api-method-summary %}
POST /messages
{% endapi-method-summary %}

{% api-method-description %}
Sends a new Message with the specified message body, to the specified Contact, using the specified Servicechain.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Authorization" type="string" required=true %}
The access token representing the User, supplied as a Bearer token
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="is\_reply\_to" type="string" required=false %}
The ID of the Message that this Message can be considered to be replying to, if any.
{% endapi-method-parameter %}

{% api-method-parameter name="contact" type="string" required=true %}
The ID of the Contact to send the Message to
{% endapi-method-parameter %}

{% api-method-parameter name="ServicechainId" type="string" required=false %}
The Servicechain that we should use to send the Message. If not supplied, then the Contact's default Servicechain will be used
{% endapi-method-parameter %}

{% api-method-parameter name="body" type="string" required=true %}
The content of the message itself
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
The message has successfully been sent.
{% endapi-method-response-example-description %}

```
{
  "id": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
  "body": "Hi! Voluble has delivered this message to you!",
  "ServicechainID": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
  "contact": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
  "is_reply_to": "c616d114-4f68-4dba-b976-cd5ae0aa359d",
  "direction": "INBOUND",
  "sent_time": 1568548864,
  "message_state": "MSG_PENDING"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

