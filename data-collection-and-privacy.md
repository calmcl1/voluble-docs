# Data Collection and Privacy

### General Principles

We don't hold any personally-identifiable information \(PII\) directly, as we believe that this sort of sensitive data handling is best left to the experts. As such, any PII that we need in order to run the service is encrypted before it leaves our system and stored with our identity provider, Auth0. Consequently, no PII is ever stored or transmitted insecurely once it is initially supplied to Voluble. This includes names, contact details, and other details that might be used to identify you in the event of a breach.



### Users

#### Consent

TODO

#### Data Storage

TODO

**Contact Information**

TODO

**Billing Information**

We haven't implemented Billing yet - this will be updated later, but will probably involve storing information with a third-party payment provider such as Stripe.

### Contacts

#### Consent

Since we don't require the collection of a Contact's personal information for a User to engage with Voluble, and we don't collect the information on a User's behalf, it is the responsibility of a User who represents an Organization to confirm that they have obtained the consent of the person represented by the Contact to collect their details and pass them on to Voluble.

#### Data Storage

Once a User has collected a Contact's PII and transferred it to Voluble, it is stored securely in the same way as a User's information.

**What We Store**

Auth0 stores all of the information we provide securely. Read more on [Auth0's security statement](https://auth0.com/security).

* ✔️ The information is stored in this place
* ❌ The information is not stored here at all

| Information | Voluble DB | Auth0 |
| :--- | :--- | :--- |
| Auth0 User ID | ✔️ | ✔️ |
| Name | ✔️ | ✔️ |
| Email Address | ✔️ | ✔️ |
| Social media usernames | ✔️ | ✔️ |
| User privileges | ❌ | ✔️ |
| User organization ID | ✔️ | ❌ |

