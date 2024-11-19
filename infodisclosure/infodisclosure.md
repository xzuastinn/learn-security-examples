# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?

1. The code is vulnerable to NoSQL injection because user input from req.query is used directly in the database query without proper sanitization or validation. An attacker can craft malicious input to manipulate the query and access or modify unauthorized data. Additionally, information disclosure is possible since sensitive user data is directly sent back in the response.

2. An attacker could exploit this by passing a specially crafted query like username[$ne]=null to bypass authentication entirely. They could also extract data about users by taking advantage of the console.log(username) statement or the response format. This leaks information that shouldn't be exposed, making the app a goldmine for attackers.

3. In secure.ts, input validation and sanitization are key. Query parameters are parsed and checked for expected types or patterns before use. For example, parameterized queries or libraries like mongoose-express-sanitizer help neutralize malicious inputs. To prevent info disclosure, only minimal, necessary data is returned to users, and error messages are vague—like "Invalid login attempt" instead of revealing what failed. These steps make exploiting harder.