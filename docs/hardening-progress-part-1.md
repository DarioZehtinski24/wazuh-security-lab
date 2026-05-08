# Wazuh Hardening Progress



## Summary



This document summarizes the security hardening work completed on the Ubuntu server running Docker and Wazuh.



The goal of this phase was to improve the Wazuh Security Configuration Assessment (SCA) score by implementing practical and low-risk CIS-aligned hardening controls while maintaining Docker and Ubuntu stability.



### Completed Hardening Tasks



* Disabled unused filesystem kernel modules

* Hardened `/tmp` mount options

* Hardened `/dev/shm` mount options

* Installed and enabled AppArmor

* Enabled AppArmor at boot

* Enforced AppArmor profiles

* Disabled core dumps

* Disabled Apport crash reporting

* Hardened local login banner

* Hardened remote login banner



### Deferred / Skipped Controls



The following CIS controls were intentionally skipped because the server currently uses a single-partition architecture and Docker workloads:



* Separate `/home` partition

* Separate `/var` partition

* Separate `/var/log` partition

* Separate `/var/log/audit partition
Additional /home and /var mount flag controls

These were considered lower-value architectural recommendations compared to the operational hardening controls already implemented.

Environment
OS: Ubuntu Server 24.04 LTS
Monitoring: Wazuh
Containers: Docker
Hardening Changes
1. Disabled Unused Filesystem Kernel Modules
Purpose

Reduce kernel attack surface and prevent unnecessary filesystem modules from loading.

Actions

Created:

/etc/modprobe.d/harden-filesystems.conf

Disabled modules:

afs
ceph
cifs
exfat
gfs2
nfsd
smbfs_common
Notes

The following modules were intentionally left enabled due to operational or Docker-related dependencies:

ext
fuse
fat
nfs_common

2. Hardened /tmp Mount
Purpose

Prevent execution of malicious binaries from temporary storage.

Actions

Added to /etc/fstab:

tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=2G 0 0
Security Benefits
noexec
nosuid
nodev
isolated temporary storage

3. Hardened /dev/shm Mount
Purpose

Prevent execution of code from shared memory.

Actions

Added to /etc/fstab:

tmpfs /dev/shm tmpfs defaults,rw,nosuid,nodev,noexec,relatime 0 0
Security Benefits
prevents executable payloads in shared memory
reduces attack surface

4. Installed and Enabled AppArmor
Purpose

Enable Mandatory Access Control (MAC) protections.

Actions

Installed:

sudo apt install apparmor apparmor-utils -y

Enabled service:

sudo systemctl enable apparmor
sudo systemctl start apparmor

Verified using:

sudo apparmor_status

5. Enabled AppArmor at Boot
Purpose

Ensure AppArmor remains active after reboot.

Actions

Modified:

/etc/default/grub

Added:

apparmor=1 security=apparmor

Updated GRUB:

sudo update-grub

6. Enforced AppArmor Profiles
Purpose

Ensure AppArmor profiles operate in enforce mode.

Actions

Executed:

sudo aa-enforce /etc/apparmor.d/*
Result
profiles enforcing
Docker AppArmor profile active

7. Disabled Core Dumps
Purpose

Prevent sensitive process memory from being written to disk.

Actions

Created:

/etc/sysctl.d/60-fs_sysctl.conf

Added:

fs.suid_dumpable = 0

Applied:

sudo sysctl -w fs.suid_dumpable=0

Created:

/etc/security/limits.d/99-hardcore.conf

Added:

* hard core 0

8. Disabled Apport Crash Reporting
Purpose

Prevent storage and reporting of sensitive crash information.

Actions

Modified:

/etc/default/apport

Set:

enabled=0

Disabled service:

sudo systemctl disable apport.service
sudo systemctl stop apport.service
Result

Wazuh marked the control as not applicable.

9. Hardened Local Login Banner
Purpose

Prevent disclosure of operating system information.

Actions

Modified:

/etc/issue

Configured:

Authorized users only. All activity may be monitored and reported.

Removed:

Ubuntu version information
kernel information
architecture details

10. Hardened Remote Login Banner
Purpose

Prevent remote OS fingerprinting.

Actions

Modified:

/etc/issue.net

Configured:

Authorized users only. All activity may be monitored and reported.

Configured SSH banner:

Banner /etc/issue.net

Restarted SSH service.

Notes

This hardening process focused on practical security improvements that:

improve the Wazuh SCA score
maintain Docker compatibility
reduce attack surface
avoid unnecessary operational risk

Future hardening tasks may include:

SSH hardening
Docker daemon hardening
UFW firewall configuration
Fail2ban
Auditd rules
Automatic security updates
Kernel sysctl tuning
Docker log restrictions
