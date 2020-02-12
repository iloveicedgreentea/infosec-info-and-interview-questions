# Attacks

## XSS
Cross-Site Scripting - a malicious script can be injected such that it runs in your browser.


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

## CSRF
CSRF is an attack that tricks a victim into performing an action without their input. An example is using some trick to run a request on a page they visit, usually under your control, which sends a request to their bank. Their cookie will be sent and then you can steal their money. This all happens under the hood.

### Mitigations
* Idempotent and read-only GET requests: GET should only read data, not write it or cause changes
* Anti-forgery tokens - You can insert hidden fields with a random token that your API will look for. Third party sites will not have this, so you can detect the false request
* Force login validation - ever notice that sometimes GH or Amazon will make you put your password in for certain actions? Part of the motivation behind that is stopping CSRF.

### SQL Injection
wip
### Mitigations
wip


## Buffer Overflows
An exploit where improper code, almost always C or C++, does not perform bounds checks on assigned buffers. An attacker can leverage this to write the memory regions beyond this bound and potentially, control code execution. They can insert other code to run and thus get full control of your system if it is over-privileged. Back to defense in depth, you can lower your exposure by restricting privileges like not running apps as root.


### Mitigations
* Memory spacing - put space between memory addresses so the overflow won't overwrite important stuff
* ASLR - Randomize the location of memory so its unpredictable
* DEP - Stop execution of specific memory sectors
* Stack canaries/cookies - Place the canary before stack return pointer. The RP would get overwritten with an overflow, so the next function can look for this value. If not present, we know a buffer overflow was attempted and we can crash. 

### Replay Attack
https://en.wikipedia.org/wiki/Replay_attack

In a MITM, you can take some communication and send it again. This can cause fake authentication to get accepted if an attacker can replay some part of the session containing a password. 

### Mitigations
Create a random session token and a sequence number, send those with the MAC