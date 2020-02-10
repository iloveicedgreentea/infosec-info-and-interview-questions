# Networking Concepts

## Communication

### TCP

### UDP

### HTTP/S


## Traffic Basics

### Firewall

* Filtered - Silently dropped. Useful if you don't want to give away that something is inaccessible. 

* Closed - Send a TCP RST or Destination Unreachable. Better for internal networks where you are comfortable giving away information about listening/non-listening ports.

A packet isn't getting to your server either way, but filtering is just one tiny extra step to make a hacker's job harder.

### Traceroute

Send a UDP (Linux) or ICMP (Windows) transmission with a TTL of 1. Until you get a destination unreachable or echo reply, increment the TTL and send a new transmission each time you get a `time exceeded` message. Not every router will send a packet back but many will. 


## Misc Info

### HTTP Codes

* 401 - You are authenticated but not authorized
* 403 - You are not authenticated but also misconfigured apps will send this instead of a 401
* 404 - Not found, keep scanning
* 429 - please stop

* 500 - The request made the server break
* 501 - I don't know what you are asking 
* 502 - Origin did not respond correctly
* 503 - Origin overloaded or dead
* 504 - Origin server timeout