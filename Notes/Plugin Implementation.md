# Plugin Implementation

-   Esendex
    -   Works in alpha stage
    -   Can send and receive messages
    -   Todo: Implement batch message support
    -   Todo: implement Esendex credit monitoring
-   Telegram
    -   Technically tricky
    -   Needs client library - is this stateful, or does it require setup/teardown _every time_?
        -   If so, batch messages are _much_ more efficient
    -   How to identify incoming messages without phone/email
-   WhatsApp
    -   Required a WhatsApp host to be run in a **separate** Docker container, or to use a PaaS third-party middleman - both of these require £££.
-   Viber?
-   Email fallback?