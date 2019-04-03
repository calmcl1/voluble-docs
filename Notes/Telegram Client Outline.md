# Telegram Client Outline

## Overview

Needs to expose an API for:

-   Sending messages (`/messages` POST?)
-   Passing system messages and receiving them (`/system` POST?)
    -   Phone code required, etc
    -   This goes to the `<voluble>/services/telegram` endpoint
    -   Should define a type - `ServiceMessage`?

## Init Flow

```flow
st=>start: Start
opSetTdLibParams=>operation: Set TdLibParameters
opCheckDBEncrKey=>operation: CheckDatabaseEncryptionKey
condPhoneCodeWait=>condition: Waiting on phone code?
opSendSvcMessage=>operation: Send `ServiceMessage` to Voluble plugin
ioAwaitPhoneCodeReq=>inputoutput: Await request with phone code
condPhoneCodeValid=>condition: Is phone code valid?
opEmitReady=>operation: Emit `ready` event
ioWaitMessageReq=>inputoutput: Wait for message request
subSendMessage=>subroutine: Send message

st->opSetTdLibParams->opCheckDBEncrKey->condPhoneCodeWait
condPhoneCodeWait(yes)->opSendSvcMessage->ioAwaitPhoneCodeReq->condPhoneCodeValid
condPhoneCodeWait(no)->opEmitReady
condPhoneCodeValid(no)->opSendSvcMessage
condPhoneCodeValid(yes)->opEmitReady
opEmitReady->ioWaitMessageReq->subSendMessage

```

