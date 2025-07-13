# 🔒 CIS Benchmarks 

This document provides security hardening rules for disabling **unused or insecure services** in compliance with CIS benchmarks. The goal is to reduce the system’s **attack surface**, enhance **stability**, and maintain **confidentiality and compliance** in both:

- ✅ Production (PROF) environments  
- ✅ Confidential/Compliance-bound (CONF) environments  

---

##  2.1 Disable Unused Services (Automated)

| Rule | Service | Action | Reason | Applicable |
|------|---------|--------|--------|------------|
| 2.1.1 | autofs | Disable | Automatically mounts file systems; can be misused for unauthorized access | PROF, CONF |
| 2.1.2 | avahi-daemon | Disable | Used for local device discovery; leaks network info | PROF, CONF |
| 2.1.3 | DHCP Server | Disable | Servers typically use static IPs; rogue DHCP can hijack traffic | PROF, CONF |
| 2.1.4 | DNS Server | Disable | Unnecessary unless the host is a DNS server; vulnerable to poisoning | PROF, CONF |
| 2.1.5 | dnsmasq | Disable | Lightweight DNS/DHCP server; may conflict with enterprise DNS | PROF, CONF |
| 2.1.6 | FTP Server | Disable | Plaintext authentication; replaced by SFTP/HTTPS | PROF, CONF |
| 2.1.7 | LDAP Server | Disable | If not needed for identity services, it's a security risk | PROF, CONF |
| 2.1.8 | Message Access Server (e.g., dovecot) | Disable | Required only for mail delivery; risk of spam relay | PROF, CONF |
| 2.1.9 | NFS Server | Disable | Can be exploited for unauthorized remote access | PROF, CONF |
| 2.1.10 | NIS Server | Disable | Obsolete protocol; vulnerable to spoofing | PROF, CONF |
| 2.1.11 | Print Server (CUPS) | Disable | Unused in most server environments; potential DoS risk | PROF, CONF |
| 2.1.12 | rpcbind | Disable | Legacy RPC support; target for exploits | PROF, CONF |
| 2.1.13 | rsync daemon | Disable | Only needed for sync services; may expose file structure | PROF, CONF |
| 2.1.14 | Samba Server | Disable | SMB protocol can leak sensitive data; not needed unless file sharing | PROF, CONF |
| 2.1.15 | SNMP | Disable | Exposes system data; vulnerable if misconfigured | PROF, CONF |
| 2.1.16 | TFTP Server | Disable | No authentication; risky protocol | PROF, CONF |
| 2.1.17 | Web Proxy Server | Disable | If not configured properly, can allow traffic interception | PROF, CONF |
| 2.1.18 | Web Server (Apache/Nginx) | Disable | Only required if serving web content | PROF, CONF |
| 2.1.19 | xinetd | Disable | Deprecated service launcher; exposes multiple ports | PROF, CONF |
| 2.1.20 | X Window System | Disable | GUI not required on servers; large attack surface | PROF, CONF |
| 2.1.21 | Mail Transfer Agent | Configure for local-only | Avoid being an open mail relay | PROF, CONF |
| 2.1.22 | Approved services only | Manual Check | Only allow necessary services to listen on network interfaces | PROF, CONF |

---

##  2.2 Configure Client Services (Automated)

| Rule | Client Package | Action | Reason | Applicable |
|------|----------------|--------|--------|------------|
| 2.2.1 | NIS Client | Remove | Obsolete and insecure | PROF, CONF |
| 2.2.2 | rsh Client | Remove | Sends data in plaintext; replaced by SSH | PROF, CONF |
| 2.2.3 | talk Client | Remove | Rarely used; unnecessary chat protocol | PROF, CONF |
| 2.2.4 | telnet Client | Remove | Plaintext protocol; major security risk | PROF, CONF |
| 2.2.5 | LDAP Client | Remove if unused | Only needed for LDAP-auth systems | PROF, CONF |
| 2.2.6 | FTP Client | Remove | Plaintext authentication; use SFTP instead | PROF, CONF |

---

## ⏱ 2.3 Configure Time Synchronization

| Rule | Component | Action | Reason | Applicable |
|------|-----------|--------|--------|------------|
| 2.3.1 | Time sync | Ensure in use | Accurate time is essential for logs, auth, certs | PROF, CONF |
| 2.3.1.1 | One daemon only | Use either `chrony` or `systemd-timesyncd` | Prevents conflict in time updates | PROF, CONF |

### If using systemd-timesyncd:
| Rule | Component | Action |
|------|-----------|--------|
| 2.3.2.1 | Configure with authorized timeserver | e.g., time.cloudflare.com |
| 2.3.2.2 | Ensure it is enabled and running |

###  If using chrony:
| Rule | Component | Action |
|------|-----------|--------|
| 2.3.3.1 | Configure with authorized timeserver |
| 2.3.3.2 | Ensure it runs as `_chrony` user |
| 2.3.3.3 | Ensure chrony is enabled and running |

---

##  2.4 Configure Job Schedulers

###  2.4.1 Cron Settings

| Rule | File/Service | Action | Reason |
|------|--------------|--------|--------|
| 2.4.1.1 | `cron` service | Enabled and active | Ensures automation tasks run |
| 2.4.1.2 | `/etc/crontab` | Set ownership and permissions | Prevent tampering |
| 2.4.1.3 | `/etc/cron.hourly` | Secure permissions | Restrict access |
| 2.4.1.4 | `/etc/cron.daily` | Secure permissions | Restrict access |
| 2.4.1.5 | `/etc/cron.weekly` | Secure permissions | Restrict access |
| 2.4.1.6 | `/etc/cron.monthly` | Secure permissions | Restrict access |
| 2.4.1.7 | `/etc/cron.d` | Secure permissions | Prevent unauthorized scheduling |
| 2.4.1.8 | `crontab` access | Restrict to authorized users | Control job creation |

### 🔧 2.4.2 At Settings

| Rule | File | Action | Reason |
|------|------|--------|--------|
| 2.4.2.1 | `/etc/at.allow` or `/etc/at.deny` | Only allow authorized users | Prevent misuse of one-time tasks |

---

## Summary

- 🔒 Disable all services that are not explicitly required.
- 🧹 Remove unused clients and protocols.
- ⏱️ Use and configure a single time synchronization service.
- 🧾 Secure cron and at job scheduling systems.
- 🎯 Apply all controls across **PROF** and **CONF** environments.

---

##  Reference

- [CIS Benchmarks – Red Hat / Ubuntu / Debian](https://www.cisecurity.org/cis-benchmarks)
- [NIST SP 800-53 Security Controls](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf)

---


