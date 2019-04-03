# Batch/Blast Messaging

-   A batch message in Voluble is called a `blast`. It's the same message body, sent to multiple recipients at the same time.
-   It shouldn't take much implementation, just the batch-sending option where available for certain plugins
    -   Is this true? What if one message fails? How is it reported back to Voluble?
    -   It may simply make more sense to loop through each message and attempt to send them individually.
        -   Though, this is inefficient and may incur rate-limiting from some services.
    -   Perhaps the best thing to do is to use batch-sending where available, then for every failed message, report back to the main process to attempt to re-send that particular message
        -   Does that incur extra cost? Does Esendex (etc) charge for failed messages?