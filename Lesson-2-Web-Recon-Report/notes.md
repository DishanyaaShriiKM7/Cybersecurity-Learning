# Lesson 2 Notes

## What to record during web reconnaissance

- Target IP and hostname
- Open HTTP/HTTPS ports
- HTTP status code
- Page title
- Server header
- Framework/CMS hints
- Redirects
- Interesting paths and directories
- Exposed files
- Virtual-host names
- Cookies and security-related headers

## Useful distinctions

**Nmap** identifies listening network services.

**WhatWeb** fingerprints web technologies from application responses.

**cURL** sends HTTP requests and exposes the response in a terminal-friendly form.

**/etc/hosts** performs local hostname-to-IP mapping and can be required for virtual-host based applications.

**Browser developer tools / Burp Suite** provide deeper visibility into individual HTTP requests and responses. Burp Suite is the next lesson.
