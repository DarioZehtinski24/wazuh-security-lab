# Troubleshooting Notes



## Networking Issues



### Problem

The Wazuh dashboard was unreachable from the host machine.



### Cause

The VM network adapter was configured as NAT.



### Fix

Changed VMware network adapter to Bridged mode.



---



## Docker Compose Issues



### Problem

`docker-compose` command was not found.



### Fix

Installed Docker Compose plugin and verified functionality.



---



## Disk Space Problems



### Problem

APT operations failed due to insufficient disk space.



### Cause

Initial VM disk size was too small.



### Fix

Expanded VMware virtual disk and resized filesystem inside Ubuntu.



---



## Wazuh API Offline



### Problem

Dashboard showed API connection as offline.



### Fix

Waited for Wazuh containers to fully initialize.



---



## Wazuh Agent Registration Failure



### Problem

Wazuh agent service failed to start.



### Cause

Incorrect manager IP variable was used.



### Fix

Configured correct manager address and restarted agent.



---



## GitHub Push Permission Error

### Problem
Git push returned HTTP 403.

### Cause
HTTPS authentication/token issues.

### Fix
Configured SSH authentication with GitHub and switched repository remote URL to SSH.
