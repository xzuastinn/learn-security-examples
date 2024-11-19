# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

## Answers

1. The endpoint is vulnerable to DoS (Denial of Service) attacks because user input (id) is used directly in a MongoDB query. Malicious attackers can send large payloads or invalid MongoDB ObjectIDs, which can crash the database or exhaust server resources. Also, there are no rate limits or input validations.

2. Attackers can flood the /userinfo route with malformed id values or excessively large queries. This could overwhelm the database, causing it to slow down or crash. Repeated requests without protections like rate limiting can also exhaust the server's memory or CPU.

3. To prevent this, input validation is critical—id should be validated against a proper MongoDB ObjectID format using libraries like mongoose.Types.ObjectId.isValid(). Rate limiting with tools like express-rate-limit can block repeated requests from the same IP. Additionally, handling invalid input gracefully prevents crashes. These steps ensure the server remains stable under attacks.