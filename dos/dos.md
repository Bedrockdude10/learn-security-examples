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

1. The primary DoS vulnerability in insecure.ts is the lack of rate limiting and input validation when handling database queries.
The application directly uses unchecked user input in MongoDB queries without proper validation. There's no mechanism to limit the number or frequency of requests a client can make, allowing an attacker to flood the server with requests. The code also lacks proper error handling for malformed queries, which can cause the server to crash or consume excessive resources when processing malicious inputs.
2. An attacker can exploit these vulnerabilities in several ways. They could send a flood of requests to the /userinfo endpoint, overwhelming the server's capacity to respond, or they could craft queries with NoSQL operators like ?id[$ne]= that force the database to scan all documents, causing high CPU and memory usage. Since there's no validation on the id parameter, an attacker could send malformed ObjectIDs or complex query objects that MongoDB needs to process, slowing down or even crashing the application. 
3. The secure.ts file implements several protective measures. The first is rate limiting with middleware to restrict the number of requests allowed from a single IP address. The secure implementation also has proper error handling. The secure version also has more specific query criteria, allowing for more efficient processing. 