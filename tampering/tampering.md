# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

1. The primary vulnerability in insecure.ts is XSS, which allows for tampering with the application's content and behavior. The application directly injects unsanitized user input into the HTML response. 
2. An attacker could exploit this by submitting malicious HTML/JavaScript in the name field. This injected code would execute in the victim's browser with the privileges of the application's origin. 
3. The secure.ts file addresses these vulnerabilities through proper input sanitization. It implements an escapeHTML function that converts potentially dangerous characters to their HTML entity equivalents, and applies this sanitization to user input before storing it in the session.