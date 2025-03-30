# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
2. Briefly explain different ways in which vulnerability can be exploited.
3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

1. The primary spoofing vulnerability in insecure.ts relates to insecure cookie and session management. The httpOnly: false setting allows client-side JavaScript to access the session cookie, making it vulnerable to cross-site scripting (XSS) attacks. Additionally, there is no sameSite attribute set for cookies. The session secret ("SOMESECRET") is hardcoded rather than being a strong, unique value provided as an environment variable or command-line argument.
2. This vulernability can be exploited in multiple ways. Since cookies aren't httpOnly, an attacker could inject malicious JavaScript that steals the session cookie and sends it to their server. Or, an attacker can create a page that automatically submits a form to the vulnerable application's sensitive endpoint, executing actions on behalf of an authenticated user. Or an attacker might be able to set a known session ID and trick a user into authenticating with it.
3. The secure implementation fixes this with multiple improvements. First, setting httpOnly: true prevents client-side JavaScript from accessing the cookie, protecting against XSS-based cookie theft. Setting sameSite: 'strict' ensures cookies are only sent in requests originating from the same site, preventing CSRF attacks. Also, the session secret is provided as a command-line argument rather than being hardcoded. The secure version also includes sanitization of user inputs, preventing XSS attacks that could be used to steal cookies.