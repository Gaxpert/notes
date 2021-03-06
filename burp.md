
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
1.  Try a login
1.  Go to `HTTP history` and `Send to intruder`
1.  In intruder clear all the fields and add the field we are testing
1.  On payload options add values to test
1.  On `Grep - Match` click the checkbox `Flag result items with responses matching these expressions`
1.  Clear if needed previous items
1.  Add response in case the payload is successful
Ex: The login form responds "password incorrect" when we have a valid user. We set the payload for possible usernames and flag the results containing "password incorrect"(all other results will be "user is incorrect")

### Testing for weak lock-out mechanism
Account lockout mechanisms should be present to mitigate brute-force login attempts. Typically applications set the threshold between 3-5 attempts.
1.  Try a login
1.  Go to `HTTP history` and `Send to intruder`
1.  In intruder clear all the fields and add the field we are testing (password in this case)
1.  On payload options add values to test
1.  On `Intruder | Options` click on `Grep - Extract`
1.  Add button
1.  Search for the keyword response of failed login. Ex: "Not logged in"
1.  Select. On `Ok` the field should automatically set
1.  Start attack

### Testing for bypassing authentication schemes
Bypassing techniques include a **direct page request**(forced browsing), **parameter modification**, **session ID prediction** and **SQL injection**.
 	
1.  Send auth and non-auth requests to comparer
1.  Verify the differences
1.  Send non-auth request to repeater
1.  Modify parameters
1.  Send to application

### Testing for browser cache weakness
Browser caching is provided to improve performance. However, when sensitive data is typed into a browser, such data can also be cached in the browser history.
If after logging out, we press **back** on the browsers history, and we are logged in, the website as a weak browser-caching protection

### Testing the account provisioning process via REST API
Account provisioning is the process of establishing and maintaining user accounts within an application. 

## Assessing Authorization Checks
### Testing for directory traversal
Directory transversal attacks are attempts to discover or forced browse to unauthorized web pages, usually designed for administrators of the application. If an application does not configure the web document root properly and does not include proper authorization checks for each page accessed, a directory traversal vulnerability could exist.

1.  Send request to intruder
1.  Add field to fuzz in the url path
1.  Select payload with paths
1.  Uncheck **urlencode** option (Depends on case)
1.  Attack

We can check a successful attack by the length of the response, which might indicate a different content than the standard error.
This works because we force browse to an area of the web application that was **unmapped**. The term **unmapped** means the application had no direct link to this secret configuration page.
### Testing for ******Local File Include (LFI)
Web servers control access to privileged files and resources through configuration settings.(ex: '/etc/passwd' on Unix).
An **LFI** attack is an attempt to access privileged files using directory traversal attacks. This attacks include different style including **dot-dot-slash attack** (../), **directory brute-forcing**, **directory climbing** or **backtracking**.
1.  Send request to intruder
1.  Add field to fuzz in the url path
1.  Select payload with paths (Ex: path: "../../../../../../../../../../etc/passwd")
1.  Uncheck **urlencode** option (Depends on case)
1.  Attack

### Testing for **Remote File Include(RFI)
**Remote File Inclusion(RFI)** is an attack attempting to access external URLs and remotely located files. The attack is possible due to parameter manipulation and lack of server-side checks.  These oversights allow parameter changes to redirect the user to locations that are not whitelisted or sanitized with proper data validation. 
1.  Set intercept on
1.  Modify page to load to an external url
1.  Forward and set intercept off

This attack works because the page parameter does not include proper data validation to ensure the values to it are legitime.

### Testing for privilege escalation
Privilege escalation attacks occur by modifying the value of the assigned role and replacing the value with another. In the event that the attack is successful, the user gains unauthorized access to resources or functionality normally restricted to administrators or more-powerful accounts.
1.  Logged in as a regular user
1.  Intercept on
1.  Modify param "uid" for ex

### Testing for insecure direct object reference
Allowing unauthorized direct access to files or resources on a system based on user-supplied
input is known as **Insecure Direct Object Reference(IDOR)** This vulnerability allows the bypassing of authorization checks placed on such files or resources. IDOR is a result of unchecked user supplied input to retrieve an object without performing authorization checks in the application code.
1.  Intercept on
1.  Find a post request with some resource
1.  Modify parameter requested (ex: "/etc/passwd")

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
1.  Send HTTP **Response** so sequencer with cookie **session id** set
1.  In sequencer select the token location (session id for ex)
1.  Start live captury
1.  After a while stop and analyze results
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
1.  Examine a GET Request and Response
1.  The absence of this flags shows that the cookie values are not protected from JavaScript manipulation
Ex with secure and HTTPOnly set:
`Set-Cookie: PHPSESSID=<session token value1. ;path=/;Secure;HttpOnly;`
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
1.  Send auth and non-auth requests to comparer
1.  If session id doesn't change, it has a session fixation vulnerability
### Testing for exposed session variables
Session variables such as tokens, cookies, or hidden form fields are used by application
developers to send data between the client and the server. Since these variables are exposed on
the client-side, an attacker can manipulate them in an attempt to gain access to unauthorized
data or to capture sensitive information.
Burp provides a feature to unhide hidden form fields
1.  Proxy ->.  Options ->.  Response Modification ->.  Check "Unhide hidden form fields"
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

## Assessing business logic
### Testing business logic
Business logic data validation errors occur due to a lack of server-side checks, especially in a sequence of events such as shopping cart checkouts.

### Unrestricted file upload - bypassing weak validation
Many applications allow for files to be uploaded for various reasons. Business logic on the server side must include checking for acceptable files; this is known as whitelisting. If such checks are weak or only address one aspect of file attributes (for example, file extensions only), attackers can exploit these weaknesses and upload unexpected file types that may be executable on the server. 

### Performing process-timing attacks
By monitoring the time an application takes to complete a task, it is possible for attackers to gather or infer information about how an application is coded. For example, a login process using valid credentials receives a response quicker than the same login process given invalid credentials. This delay in response time leaks information related to system processes

### Testing for the circumvention of work flows
Shopping cart to payment gateway interactions must be tested by web app penetration testers to ensure the workflow cannot be performed out of sequence. A payment should never be made unless a verification of the cart contents is checked on the server-side first. In the event this check is missing, an attacker can change the price, quantity, or both, prior to the actual purchase.
