# Security Improvement Plan



This document tracks planned improvements, hardening steps, and future experiments in the Wazuh security lab.



---



# Current Priorities



## 1. Verify File Integrity Monitoring (FIM)



Goal:

- Confirm file modification alerts are working correctly

- Identify which directories are currently monitored



Planned tests:

- Modify `/etc/hosts`

- Create temporary executable files

- Observe generated alerts



Status:

- In progress



---



## 2. Improve Security Configuration Assessment (SCA) Score



Goal:

- Increase Ubuntu hardening score

- Reduce failed CIS benchmark checks



Potential improvements:

- Install AIDE

- Configure auditd

- Review file permissions

- Remove unnecessary services



Status:

- Planned



---



## 3. Vulnerability Detection



Goal:

- Verify CVE detection functionality

- Identify vulnerable packages



Planned actions:

- Explore Threat Intelligence section

- Install additional software for testing

- Observe vulnerability reporting 
Status:
- Not started

---

## 4. Threat Hunting Practice

Goal:
- Learn event investigation workflows
- Build familiarity with Wazuh queries and filters

Planned tests:
- SSH authentication failures
- Privilege escalation activity
- Process execution monitoring

Status:
- In progress

---

## 5. Custom Monitoring and Detection

Goal:
- Create custom monitors and anomaly detectors

Planned experiments:
- Failed SSH login thresholds
- High-severity alert monitoring
- File change monitoring

Status:
- In progress

---

# Future Expansion Ideas

- Add a second monitored VM
- Simulate attacker techniques
- Enable advanced malware detection
- Integrate VirusTotal
- Create custom Wazuh rules
- Explore MITRE ATT&CK mappings
- Build attack timeline scenarios

---

# Documentation Goals

Future documentation will include:

- Hardening steps performed
- Before/after SCA score comparison
- Alert screenshots
- Detection explanations
- Attack simulation notes
- Lessons learned
