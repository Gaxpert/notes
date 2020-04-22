
***
title: webapps
date: 2020-04-22 13:33:15+01:00
author: Gaxpert
***


# webapps
## Core defense mechanisms
### Authentication
* Involves the process of establishing that the user is who he claims he is.
* Usually enforced through login and password

### Session Management
* Process of handling user access.
* Almost all web applications use **tokens**.
* Token is a unique string that the application maps to a session
* HTTP cookies are the standard method for transmitting session tokens, although some web apps use hidden form fields or the URL query string

### Access Control
* Make the correct decisions about what data each user can access

## Approaches to input handling
### "Reject known bad"
This approach typically employs a blacklist of string or patterns commonly used in attacks. The validating mechanism blocks these patterns by default. Its regarded as the least useful approach:
* A typical vulnerability can be exploited by a wide variety of ways
* Exploits are constantly evolving.
Finally, many blacklist approaches can be bypassed by inserting a null byte at the start of the exploit. That way the blacklist filter will stop processing the string because it thinks the string is finished
`%00<script>alert(1)</script>`
