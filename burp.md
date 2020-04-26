
***
title: burp
date: 2020-04-25 17:03:40+01:00
author: Gaxpert
***


# burp

## Configuration
Set firefox proxy on manual and on ip `127.0.0.1` and port 8080

### Https
Go to [burp](http://burp) and download the certificate. Then add certificate to firefox (import) with trust for websites

## Assessing Authentication Schemes
### Username enumeration
Some login forms might return "password is invalid" or "user is invalid". If the response is dependent on one of the forms being correct/incorrect we can enumerate correct values
> Try a login
> Go to `HTTP history` and `Send to intruder`
> In intruder clear all the fields and add the field we are testing
> On payload options add values to test
> On `Grep - Match` click the checkbox `Flag result items with responses matching these expressions`
> Clear if needed previous items
> Add response in case the payload is successful
Ex: The login form responds "password incorrect" when we have a valid user. We set the payload for possible usernames and flag the results containing "password incorrect"(all other results will be "user is incorrect")

### Testing for weak lock-out mechanism
Account lockout mechanisms should be present to mitigate brute-force login attempts. Typically applications set the threshold between 3-5 attempts.
> Try a login
> Go to `HTTP history` and `Send to intruder`
> In intruder clear all the fields and add the field we are testing (password in this case)
> On payload options add values to test
> On `Intruder | Options` click on `Grep - Extract`
> Add button
> Search for the keyword response of failed login. Ex: "Not logged in"
> Select. On `Ok` the field should automatically set
> Start attack

### Testing for bypassing authentication schemes
Bypassing techniques include a **direct page request**(forced browsing), **parameter modification**, **session ID prediction** and **SQL injection**.
 	
> Send auth and non-auth requests to comparer
> Verify the differences
> Send non-auth request to repeater
> Modify parameters
> Send to application

### Testing for browser cache weakness
Browser caching is provided to improve performance. However, when sensitive data is typed into a browser, such data can also be cached in the browser history.
If after logging out, we press **back** on the browsers history, and we are logged in, the website as a weak browser-caching protection

### Testing the account provisioning process via REST API
Account provisioning is the process of establishing and maintaining user accounts within an application. 

## Assessing Authorization Checks
### Testing for directory traversal
Directory transversal attacks are attempts to discover or forced browse to unauthorized web pages, usually designed for administrators of the application. If an application does not configure the web document root properly and does not include proper authorization checks for each page accessed, a directory traversal vulnerability could exist.

> Send request to intruder
> Add field to fuzz in the url path
> Select payload with paths
> Uncheck **urlencode** option (Depends on case)
> Attack

We can check a successful attack by the length of the response, which might indicate a different content than the standard error.
This works because we force browse to an area of the web application that was **unmapped**. The term **unmapped** means the application had no direct link to this secret configuration page.
### Testing for ******Local File Include (LFI)
Web servers control access to privileged files and resources through configuration settings.(ex: '/etc/passwd' on Unix).
An **LFI** attack is an attempt to access privileged files using directory traversal attacks. This attacks include different style including **dot-dot-slash attack** (../), **directory brute-forcing**, **directory climbing** or **backtracking**.
> Send request to intruder
> Add field to fuzz in the url path
> Select payload with paths (Ex: path: "../../../../../../../../../../etc/passwd")
> Uncheck **urlencode** option (Depends on case)
> Attack

### Testing for **Remote File Include(RFI)
**Remote File Inclusion(RFI)** is an attack attempting to access external URLs and remotely located files. The attack is possible due to parameter manipulation and lack of server-side checks.  These oversights allow parameter changes to redirect the user to locations that are not whitelisted or sanitized with proper data validation. 
> Set intercept on
> Modify page to load to an external url
> Forward and set intercept off

This attack works because the page parameter does not include proper data validation to ensure the values to it are legitime.

### Testing for privilege escalation
Privilege escalation attacks occur by modifying the value of the assigned role and replacing the value with another. In the event that the attack is successful, the user gains unauthorized access to resources or functionality normally restricted to administrators or more-powerful accounts.
> Logged in as a regular user
> Intercept on
> Modify param "uid" for ex

### Testing for insecure direct object reference
Allowing unauthorized direct access to files or resources on a system based on user-supplied
input is known as **Insecure Direct Object Reference(IDOR)** This vulnerability allows the bypassing of authorization checks placed on such files or resources. IDOR is a result of unchecked user supplied input to retrieve an object without performing authorization checks in the application code.
> Intercept on
> Find a post request with some resource
> Modify parameter requested (ex: "/etc/passwd")

## Assesiing Session Management Mechanisms
### Testing session token strength using Sequencer
To track user activity from page to page within an application, developers create and assign
unique session token values to each user. Most session token mechanisms include session IDs,
hidden form fields, or cookies. Cookies are placed within the user's browser on the client-side.
These session tokens should be examined by a penetration tester to ensure their uniqueness,
randomness, and cryptographic strength, to prevent information leakage.
If a session token value is easily guessable or remains unchanged after login, an attacker could
apply (or fixate) a pre-known token value to a user. This is known as a **session fixation attack**.
Generally speaking, the purpose of the attack is to harvest sensitive data in the user's account,
since the session token is known to the attacker.
> Send HTTP **Response** so sequencer with cookie **session id** set
> In sequencer select the token location (session id for ex)
> Start live captury
> After a while stop and analyze results
Burp will send multiple requests extracting the session id cookie for entropy analysis
For more info on tests: [Burp sequencer](https://portswigger.net/burp/documentation/desktop/tools/sequencer/tests)

### Testing for cookie attributes
Important user-specific information, such as session tokens, is often stored in cookies within the
client browser. Due to their importance, cookies need to be protected from malicious attacks. This
protection usually comes in the form of two flags: **secure** and **HttpOnly**.
The **secure** flag informs the browser to only send the cookie to the web server if the protocol is
encrypted (for example, HTTPS, TLS). This flag protects the cookie from eavesdropping over
unencrypted channels.
The **HttpOnly** flag instructs the browser to not allow access or manipulation of the cookie via
JavaScript. This flag protects the cookie from cross-site scripting attacks.
> Examine a GET Request and Response
> The absence of this flags shows that the cookie values are not protected from JavaScript manipulation
Ex with secure and HTTPOnly set:
`Set-Cookie: PHPSESSID=<session token value>;path=/;Secure;HttpOnly;`
### Testing for session fixation
Session tokens are assigned to users for tracking purposes. This means that when browsing an
application as unauthenticated, a user is assigned a unique session ID, which is usually stored in
a cookie. Application developers should always create a new session token after the user logs into
the website. If this session token does not change, the application could be susceptible to a
session fixation attack. It is the responsibility of web penetration testers to determine whether
this token changes values from an unauthenticated state to an authenticated state.
Session fixation is present when application developers do not invalidate the unauthenticated
session token, allowing the user to use the same one after authentication. This scenario allows an
attacker with a stolen session token to masquerade as the user.
> Send auth and non-auth requests to comparer
> If session id doesn't change, it has a session fixation vulnerability
### Testing for exposed session variables
Session variables such as tokens, cookies, or hidden form fields are used by application
developers to send data between the client and the server. Since these variables are exposed on
the client-side, an attacker can manipulate them in an attempt to gain access to unauthorized
data or to capture sensitive information.
Burp provides a feature to unhide hidden form fields
> Proxy -> Options -> Response Modification -> Check "Unhide hidden form fields"
### Testing for Cross-Site Request Forgery
**Cross-Site Request Forgery (CSRF)** is an attack that rides on an authenticated user's session
to allow an attacker to force the user to execute unwanted actions on the attacker's behalf. The
initial lure for this attack can be a phishing email or a malicious link executing through a cross
site scripting vulnerability found on the victim's website. CSRF exploitation may lead to a data
breach or even a full compromise of the web application.

CSRF attacks require an authenticated user session to surreptitiously perform actions within the
application on behalf of the attacker. In this case, an attacker rides on ed's session to re-run the
registration form, to create an account for the attacker. If ed had been an admin, this could have
allowed the account role to be elevated as well.



