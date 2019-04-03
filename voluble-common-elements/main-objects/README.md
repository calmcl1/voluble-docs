# Main Objects

Voluble uses these objects to represent the fundamental concepts that it deals with.

The primary purpose that Voluble fulfils is to send Messages to people \('[Contacts](contact.md)'\), using a variety of methods. It does this by interfacing with third-party platforms \(called '[Services](service.md)'\) that handle message delivery.

If a Message cannot be delivered by a certain Service \(the Contact has not signed up for Telegram, for example,\) then Voluble will hand the Message to another Service to attempt to deliver it that way, until the Message is successfully delivered, or Voluble runs out of Services to try. This failover-chain is called a '[Servicechain](servicechain.md)' \(i.e. a chain of Services to use\).

Voluble is designed such that more than one person can send messages from a given phone number - in that an '[Organization](organization.md)' \(a company or group that uses a certain phone number for its communications\) can have more than one '[User](user.md)', so different departments or members of staff can all have access to Voluble.

Voluble also has the ability to send the same message to more than one person at a time - this mass-messaging system is called a '[Blast](blast.md)'.

