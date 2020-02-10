# Attacks




## Buffer Overflows
An exploit where improper code, almost always C or C++, does not perform bounds checks on assigned buffers. An attacker can leverage this to write the memory regions beyond this bound and potentially, control code execution. They can insert other code to run and thus get full control of your system if it is over-privileged. Back to defense in depth, you can lower your exposure by restricting privileges like not running apps as root.


### Mitigations

* Memory spacing - put space between memory addresses so the overflow won't overwrite important stuff
* ASLR - Randomize the location of memory so its unpredictable
* DEP - Stop execution of specific memory sectors
* Stack canaries/cookies - Place the canary before stack return pointer. The RP would get overwritten with an overflow, so the next function can look for this value. If not present, we know a buffer overflow was attempted and we can crash. 

### Replay Attack
https://en.wikipedia.org/wiki/Replay_attack

In a MITM, you can take some communication and send it again. This can cause fake authentication to get accepted if an attacker can replay some part of the session containing a password. This is mitigated with a session token and a sequence number.