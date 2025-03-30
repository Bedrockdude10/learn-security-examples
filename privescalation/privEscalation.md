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

1. The primary vulnerability in insecure.ts is an improper authentication and authorization mechanism that allows for privilege escalation. The application performs authentication based solely on user ID provided in the request body. Additionally, authorization is performed based on this self-reported identity. There is also no session management or proper authentication mechanism to verify that the requester is actually the user they claim to be. Finally, the code directly modifies the user's role without any additional verification. 
2. An attacker can exploit these vulnerabilities through several methods. They could submit a request with the userId of an admin account (in this case, userId=1), allowing the attacker to impersonate the admin user without knowing any credentials. Once authenticated as admin, the attacker could update the role of any user, including themself, to gain additional privileges. Since there's no CSRF protection, an attacker could even trick an authenticated admin into submitting such requests unknowingly through a malicious website.
3. The secure.ts file implements several important protective measures. First, it uses express-session to maintain server-side session state and checks if the user is logged in through the session. It also verifies the role of the authenticated user based on their session and not the request parameters. It also has secure cookie settings enabled to prevent CSRF attacks. 