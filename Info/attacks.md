# Attacks

## Credential Stuffing
This is fundamentally tied to password reuse. An attacker can automate login forms using known password/email combos. It is not brute-forcing the passwords per se, just trying previously cracked/stolen combinations. 

### Mitigations

* MFA - Enforce real (non-sms) multi-factor authentication on all login attempts that do not have a cookie present. SMS is not real MFA. 

* Account lockouts - good in general but won't really help unless they try multiple passwords for one user. Still, you should have them regardless.

* Checking password hashes on creation - You can use the [haveibeenpwned api](https://haveibeenpwned.com/API/v2) to look up partial hashes without exposing the entire hash. This will prevent reuse of a breached password.

* Enforce good passwords - most breached passwords are not very secure.

* Password Managers - Educate clients and people about password managers. All passwords should be unique, long, and random.

## XSS
* Cross-Site Scripting

XSS is an attack where a script or similar information can be injected such that it runs in your browser.

### Static/Stored
An attacker can inject some JS into a comment which gets stored in the database. Subsequent visits to the form/page will result in code execution.

### Reflected
You can craft a URL to a site which accepts some kind of input, and if a user clicks on that link, the website (trusted) will return data that the browser will execute. 

```
https://example.com/get_page?data=<script>window.alert(document.cookie)</script> 
```

If a user clicks this link, their browser will trust it as it is coming from a "trusted" website.

### Mitigations

* Sanitize input. All major frameworks have a library or function to do this
* URL safe encode all special characters
* JS/HTML escape data
* Use content security policies to block inline and external scripts from domains not whitelisted
* Phishing training and protections

## CSRF
* Client-side request forgery

CSRF is an attack that tricks a victim into performing an action without their input. An example is using some trick to run a request on a page they visit, usually under your control, which sends a request to their bank. Their cookie will be sent and then you can steal their money. This all happens under the hood.

### Mitigations
* Idempotent and read-only GET requests: GET should only read data, not write it or cause changes
* Anti-forgery tokens - You can insert hidden fields with a random token that your API will look for. Third party sites will not have this, so you can detect the false request
* Force login validation - ever notice that sometimes GH or Amazon will make you put your password in for certain actions? Part of the motivation behind that is stopping CSRF.

## SSRF
* Server-side request forgery

SSRF is an attack that tricks a server into making a request on the attacker's behalf. This can allow you to access internal services, such as AWS metadata, externally. SSRF isn't just limited to HTTP requests. A common example is a web app that accepts a URL parameter - `https://example.com/service?url=http://localhost:8080/secretdata`

Depending on the app, you can make protocol requests to anything, like redis. For a good example, read about [the Capital One breach](https://blog.appsecco.com/an-ssrf-privileged-aws-keys-and-the-capital-one-breach-4c3c2cded3af).

[Here are some ways to bypass SSRF protections](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery)

### Mitigations

* Most cloud providers, finally including AWS, have an authorization scheme for metadata. AWS metadata V2 requires extra work to enable.

* Automated and manual validation of code - Check for bugs in validation or whitelists

* If you must allow URL parameters, whitelist domains/IPs and/or escape input and validate at the endpoint.


## SQL Injection
Web apps written in a variety of languages, usually PHP, sometimes pass input into SQL queries. You can just comment out the rest of the query and put your own data in.

Some examples:

* `name’ OR ‘a’=’a` - this will nullify a `WHERE` statement
* `name’); DELETE FROM items; –` - This will create another statement to delete data and comment out the rest of the query

### Mitigations
Mitigation is very simple. Almost all languages that deal with databases have some form of `prepared statements`. This will turn SQL input into literal strings. You can also use stored procedures and parameterize them, however the downside is those are stored in the DB and not in the application code.


## Buffer Overflows
An exploit where improper code, almost always C or C++, does not perform bounds checks on assigned buffers. An attacker can leverage this to write the memory regions beyond this bound and potentially, control code execution. They can insert other code to run and thus get full control of your system if it is over-privileged. Back to defense in depth, you can lower your exposure by restricting privileges like not running apps as root.

### Mitigations
* Memory spacing - put space between memory addresses so the overflow won't overwrite important stuff
* ASLR - Randomize the location of memory so its unpredictable
* DEP - Stop execution of specific memory sectors
* Stack canaries/cookies - Place the canary before stack return pointer. The RP would get overwritten with an overflow, so the next function can look for this value. If not present, we know a buffer overflow was attempted and we can crash. 

## Replay Attack
https://en.wikipedia.org/wiki/Replay_attack

In a MITM, you can take some communication and send it again. This can cause fake authentication to get accepted if an attacker can replay some part of the session containing a password. 

### Mitigations
Create a random session token and a sequence number, send those with the MAC
