# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

1. The code has a privilege escalation vulnerability because it relies on insecure authentication and authorization. The user role isn't validated against a secure session or token, allowing attackers to spoof or manipulate requests to escalate privileges.

2. An attacker can bypass the admin check by directly sending a POST request with another user's userId and a new role. Since there's no session-based authentication or role verification, they could promote themselves to admin.

3. Use secure authentication mechanisms like JWTs or session cookies to validate user identity. Implement proper role-based access control (RBAC) to ensure only admins can update roles. Additionally, validate inputs strictly and avoid directly trusting client-provided data. Without these, the app is wide open.