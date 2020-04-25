
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

### "Accept known good"
Create a whitelist containing a set of of strings and patterns known to match only benign input. For example, we may only accept alphanumeric strings for a certain field. The downside is that sometimes we need to accept characters that may be used in a malicious way.

### Sanitization
Instead of rejecting data that can not be guarenteed as safe, we can remove certain characters or HTML encode them in order to avoid exploitation. This is generally the route chosen for applications that must accept a wide range of input.

### Safe Data Handling
Many web application vulnerabilities arise from the processing of the user-supplied data. If we ensure all the processing works on a safe way, we can avoid common problems. The downside is that sometimes is hard to do that for all processing features of the web application

### Semantic Checks
The previous defenses focused on malicious crafted input, but sometimes the malicious action is similar to a normal user action. For example, if an attacker tries to gain access to a different user account by changing the account id in an hidden form field, we should have checks in place to validate that the account belongs to the correct user

### Boundary Validation
A common problem in web applications is that developers create some input checks on the client side, and then assume the data that arrives to the server is "trusted", which is not always the case. A effective model uses **boundary validation**. Each individual component or functional unit of the server side application treats its input as coming from a potentially untrusted source

###Handling attackers
* Handling errors
* Maintaining audit logs
* Alerting administrators
* Reacting to attacks

