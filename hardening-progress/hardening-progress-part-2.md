# Wazuh CIS Hardening - Ubuntu Home Lab



## Overview



This document summarizes the security hardening steps performed on an Ubuntu Server VM monitored by Wazuh. The goal of this project was to improve the Wazuh Security Configuration Assessment (SCA) score while maintaining a stable and functional Docker + Wazuh environment.



The system is a home lab environment running:

- Ubuntu Server

- Docker

- Wazuh (Docker deployment)



The hardening actions below were reviewed carefully to avoid breaking:

- Docker networking

- Wazuh services

- SSH access

- General VM functionality



--------------------------------------------------



# Quick Summary of Fixes



- Disabled or removed insecure legacy services (rsync, telnet, ftp)

- Verified and stabilized NTP time synchronization

- Hardened cron configuration files and directories

- Restricted cron and at scheduler access

- Disabled unused kernel network protocol modules (dccp, tipc, rds, sctp)

- Disabled unnecessary packet forwarding

- Disabled ICMP redirects and secure redirects

- Enabled reverse path filtering (rp_filter)

- Disabled source-routed packets

- Enabled martian packet logging

- Reviewed IPv6 router advertisement hardening

- Validated firewall configuration using UFW

- Improved overall Wazuh CIS/SCA compliance score

--------------------------------------------------
# Security Improvements Applied

## 1. Disabled or Removed Insecure Services and Clients

### rsync Service
- Disabled the rsync.service
- Reduced unnecessary attack surface
- Avoided use of the unencrypted rsync daemon protocol

### Telnet Client
Removed:
- telnet
- inetutils-telnet

Reason:
- Telnet transmits credentials unencrypted
- SSH is used instead

### FTP Client
Removed:
- ftp
- tnftp

Reason:
- FTP is insecure and unencrypted
- Reduced unnecessary legacy software

--------------------------------------------------

# Time Synchronization Hardening

## 2. Verified NTP Synchronization

### systemd-timesyncd
Confirmed that:
- NTP synchronization is active
- System clock synchronization is functioning correctly

Purpose:
- Accurate timestamps for logs
- Reliable Wazuh alerts
- Better forensic analysis

### chrony Cleanup
- Continued using systemd-timesyncd
- Prevented conflicts between multiple NTP services
- Removed unnecessary chrony-related findings

--------------------------------------------------

# Cron and Scheduled Task Hardening

## 3. Secured Cron Configuration Files and Directories

### Secured /etc/crontab

Permissions:
- Owner: root:root
- Mode: 600

### Secured Cron Directories

Restricted:
- /etc/cron.hourly
- /etc/cron.daily
- /etc/cron.weekly
- /etc/cron.monthly
- /etc/cron.d

Permissions:
- Owner: root:root
- Mode: 700

### cron.allow
Configured:
- /etc/cron.allow
- Restricted cron scheduling access
- Added authorized users only
- Protected file permissions

### at.allow
Configured:
- /etc/at.allow
- Restricted use of the at scheduler
- Applied secure ownership and permissions

Purpose:
- Prevent unauthorized scheduled tasks
- Reduce privilege escalation risks
- Protect scheduled job configuration

--------------------------------------------------
# Kernel Module Hardening

## 4. Disabled Unused Network Protocol Modules

Disabled:
- dccp
- tipc
- rds
- sctp

Purpose:
- Reduce kernel attack surface
- Remove unused networking protocols
- Lower exposure to kernel-level vulnerabilities

Hardening method:
- Added blacklist entries in /etc/modprobe.d/
- Prevented module loading using:
  install <module> /bin/false
- Attempted unloading using:
  modprobe -r
  rmmod

--------------------------------------------------

# Network Stack Hardening

## 5. Packet Forwarding Review

Reviewed:
- net.ipv4.ip_forward
- net.ipv6.conf.all.forwarding

Note:
IPv4 forwarding was preserved where necessary for Docker networking compatibility.

--------------------------------------------------

## 6. Disabled ICMP Redirect Sending

Configured:
- net.ipv4.conf.all.send_redirects = 0
- net.ipv4.conf.default.send_redirects = 0

Purpose:
- Prevent malicious route manipulation
- Reduce routing attack surface

--------------------------------------------------

## 7. Disabled Secure ICMP Redirects

Configured:
- net.ipv4.conf.all.secure_redirects = 0
- net.ipv4.conf.default.secure_redirects = 0

Purpose:
- Prevent route changes from compromised gateways
- Improve routing security

--------------------------------------------------

## 8. Enabled Reverse Path Filtering

Configured:
- net.ipv4.conf.all.rp_filter = 1
- net.ipv4.conf.default.rp_filter = 1

Purpose:
- Protect against spoofed packets
- Validate incoming packet routing paths

--------------------------------------------------

## 9. Disabled Source Routed Packets

Configured:
- net.ipv4.conf.all.accept_source_route = 0
- net.ipv4.conf.default.accept_source_route = 0
- net.ipv6.conf.all.accept_source_route = 0
- net.ipv6.conf.default.accept_source_route = 0

Purpose:
- Prevent attackers from specifying packet routes manually
- Improve routing integrity

--------------------------------------------------

## 10. Enabled Martian Packet Logging

Configured:
- net.ipv4.conf.all.log_martians = 1
- net.ipv4.conf.default.log_martians = 1

Purpose:
- Log suspicious or unroutable packets
- Improve Wazuh visibility
- Detect spoofed traffic attempts

--------------------------------------------------

# IPv6 Router Advertisement Review

## 11. IPv6 Router Advertisement Evaluation

Reviewed:
- net.ipv6.conf.all.accept_ra
- net.ipv6.conf.default.accept_ra

Decision:
- Not disabled

Reason:
- The VM actively uses IPv6 router advertisements
- Disabling RA could break IPv6 connectivity
- Preserved Docker and network stability

--------------------------------------------------

# Firewall Review

## 12. Firewall Configuration Validation

Verified:
- UFW is active and enabled
- nftables is installed but inactive
- No conflicting firewall managers are active

Current firewall posture:
- SSH denied
- HTTP denied
- HTTPS allowed

No major firewall modifications were made to avoid disrupting Docker networking.

--------------------------------------------------

# Result

The system security posture was significantly improved while preserving:
- Docker functionality
- Wazuh monitoring
- Stable networking
- SSH access

This project demonstrates practical Linux hardening using:
- CIS benchmark recommendations
- Wazuh SCA assessments
- Real-world operational considerations

The focus was not only improving the compliance score, but also understanding:
- Why each recommendation exists
- What security risk it mitigates
- Whether it is safe to apply in a real environment

--------------------------------------------------

# Tools and Technologies

- Ubuntu Server
- Docker
- Wazuh
- UFW Firewall
- systemd-timesyncd
- Linux sysctl hardening
- CIS Benchmark recommendations

--------------------------------------------------
# Notes

Some CIS findings were intentionally not remediated if:
- They risked breaking Docker networking
- They reduced system functionality unnecessarily
- They were not relevant for the home lab environment

This approach prioritized:
1. System stability
2. Practical security
3. Understanding Linux hardening concepts
4. Safe incremental changes
