# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
2. Briefly explain why the vulnerability is addressed in __secure.ts__.
3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

1. The /send-message and /get-messages routes lack mechanisms to ensure accountability. There is no logging or verification of actions, leaving the system vulnerable to repudiation. A user could deny having sent a message or accessed messages, and the system would have no proof to counter the claim.

2.In secure.ts, repudiation is addressed by adding authentication and logging mechanisms. Authentication ensures every action is tied to a verified user, while logs record user actions (e.g., timestamps, user IDs) to provide a reliable audit trail. This prevents users from denying their activities.

3. The Audit Logging pattern is used in the secure version. This pattern ensures that every user action (e.g., sending or retrieving messages) is recorded with details such as user identity, timestamps, and action type. These logs create an immutable record, enabling non-repudiation by linking actions to specific users.