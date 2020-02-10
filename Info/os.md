# OS 

## Hardening 

### First steps to securing a Linux server

This is mostly theoretical in that config management should take care of this, and most of these settings can be pre-baked in something like an AMI

* Run all updates
* Disable password based login for SSH and root login
* (Optional) Change SSH port. This won't "protect you" but will lessen the volume of your logs when skids probe it
* Install and enable a firewall. Allow only the ports you absolutely need. Don't respond to pings from the outside.
* Install something like fail2ban
* Set up centralized logging and alerting
* Very no extra services are running (nfs, telnet, dns, etc)