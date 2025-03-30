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

1. There are multiple repudiation risks in the insecure implementation. The application does not log user actions or requests at all; there's no recording of who performed which actions, when they were performed, or from which IP address they originated, since the message storage system doesn't maintain any metadata about when messages were created or modified. There's no authentication identifiers are preserved alongside actions, so even if logging were added later, it would be difficult to attribute actions to specific users. Additionally, error conditions are simply logged to console rather than being persistently stored for later analysis.
2. The secure implementation addresses these repudiation vulnerabilities by implementing comprehensive logging and auditing mechanisms. It creates a persistent log stream that records all actions to a file and implements middleware that logs every request with timestamps, method, URL, and IP address. 
3. The secure version implements the Observer Pattern combined with the Decorator Pattern for logging. The logging system acts as an observer that monitors system events without interfering with the primary functionality. Each significant action in the application triggers a notification to the logging system. The middleware implementation decorates each request handler with additional logging functionality without modifying the core request processing logic. This design pattern works by applying a logging middleware that intercepts all requests before they reach their handlers, recording metadata about each request (timestamp, IP, method, URL), decorating specific actions with additional logging statements. This ensures all errors are caught and logged with proper context, providing a centralized, consistent logging format and mechanism. This makes it difficult to successfully repudiate actions, since there is an audit trail of who did what and when. 