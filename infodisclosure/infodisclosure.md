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

1. The application directly uses unvalidated user input in the MongoDB query without any sanitization or input validation. Specifically, when handling the /userinfo endpoint, it takes the username parameter from the request query and directly inserts it into the MongoDB findOne query.
This approach is vulnerable because NoSQL databases like MongoDB accept query operators within objects, allowing for complex queries beyond simple equality checks. 
2. An attacker can exploit this vulnerability by sending specially crafted query parameters that transform the intended query into a more permissive one. 
3. The secure implementation sanitizes the input by replacing any non-alphanumeric characters in the input string, preventing injection of MongoDB operators. It also implements type checking; it verifies that the username is a string type before processing. Finally, it properly catches and logs errors without exposing sensitive information to the client.