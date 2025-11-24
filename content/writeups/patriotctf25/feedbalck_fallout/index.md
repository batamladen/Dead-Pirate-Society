---
title: "Feedbalck Fallout"
author: "Salochi"
layout: "single"
github: "https://pointerpointer.com/"
---

## Walkthrough 

#### Inspect the Front End:
The single-page form in index.html submits feedback via AJAX to /feedback and renders the success or status value returned by the servlet. No page navigation occurs, so monitoring network traffic in the browser dev tools (Network tab) is the easiest way to view responses.


#### Review the Backend:

FeedbackServlet logs the submitted feedback with Log4j:

logger.info("[SESSION:{}] User feedback: {}", sessionId, input);
Log messages matching the user’s session ID are read back from /var/log/app/app.log, escaped, and included in the JSON response. This means any string that Log4j writes will be reflected to the client.


#### Spot the Vulnerability:

Log4j 2.14.1 still resolves lookups such as ${env:VAR_NAME} inside log messages by default. When the attacker submits ${env:SECRET_FLAG}, Log4j will replace it with the process environment variable SECRET_FLAG before writing to the log file.

#### Exploit Steps:

Open browser developer tools (Network tab) or use a proxy like Burp/ZAP.
Submit the following feedback payload (URL-encoded if necessary):
feedback=${env:SECRET_FLAG}
The servlet logs the message, Log4j expands ${env:SECRET_FLAG} to the actual flag, and the file append operation writes the resolved value.
The servlet immediately filters the log file for the active session and returns it inside the JSON response.
Retrieve the Flag:

Observe the AJAX response payload in the Network tab. The logs field contains the log line with the resolved flag:
{
  "status": "success",
  "logs": "[2025-02-01 12:34:56] [SESSION:ABC123] User feedback: FLAG{…}"
}
Extract the value shown in place of ${env:SECRET_FLAG}—that is the secret flag