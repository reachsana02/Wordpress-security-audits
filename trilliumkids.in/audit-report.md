# Security Audit Report
## trilliumkids.in
**Date:** 23 March 2026  
**Auditor:** Sana Fathima  
**Classification:** Confidential

---

## Executive Summary
A security audit was conducted on trilliumkids.in, 
a children's education website built on WordPress. 
The audit identified 10 security findings across 
HIGH, MEDIUM, and LOW severity levels. All HIGH 
severity findings were fully remediated during 
the audit engagement. Two MEDIUM findings could 
not be remediated due to plugin licensing 
constraints and have been documented with 
recommendations.

**Overall Risk Before Audit:** HIGH  
**Overall Risk After Audit:** LOW-MEDIUM

---

## Scope
- Target: https://trilliumkids.in
- Type: WordPress website
- Hosting: Hostinger (LiteSpeed/hcdn)
- Testing type: Grey-box (auditor has admin access)
- In scope: WordPress core, plugins, themes, 
  server configuration, headers, authentication

---

## Methodology
OWASP Top 10 Web Application Security Risks (2021)
- A01 Broken Access Control
- A02 Cryptographic Failures  
- A05 Security Misconfiguration
- A06 Vulnerable and Outdated Components
- A07 Identification and Authentication Failures

---

## Findings Summary

| ID | Title | Severity | Status |
|----|-------|----------|--------|
| H1 | User Enumeration | HIGH | Fixed |
| H2 | REST API User Exposure | HIGH | Fixed |
| H3 | XML-RPC Enabled | HIGH | Fixed |
| M1 | Missing Security Headers | MEDIUM | Fixed |
| M2 | HTTP Before HTTPS | MEDIUM | Fixed |
| M3 | WPBakery Outdated | MEDIUM | Cannot Fix |
| M4 | Slider Revolution Outdated | MEDIUM | Cannot Fix |
| M5 | WP Version Disclosed | MEDIUM | Fixed |
| M6 | WAF in Learning Mode | MEDIUM | Fixed |
| L1 | readme.html Accessible | LOW | Fixed |
| L2 | PHP Version Disclosed | LOW | Fixed |
| L3 | VirusTotal Flag | LOW | Monitoring |
| L4 | DISALLOW_FILE_EDIT Missing | LOW | Fixed |
| L5 | No 2FA on Admin | LOW | Fixed |

---

## Detailed Findings

### [HIGH] H1: User Enumeration via Author Pages
- **URL:** /?author=1, /?author=2  
- **Exposed Data:** reacharmaanahmed, sadiq  
- **Risk:** Attackers can enumerate valid usernames, enabling targeted brute-force attacks  
- **Remediation:** Redirect rules implemented via .htaccess  
- **Status:** FIXED  

---

### [HIGH] H2: REST API User Data Exposure
- **URL:** /wp-json/wp/v2/users  
- **Exposed Data:** Usernames, IDs, avatar URLs  
- **Risk:** Unauthenticated endpoint allows direct user harvesting  
- **Remediation:** Endpoint disabled using WPCode PHP snippet  
- **Status:** FIXED  

---

### [HIGH] H3: XML-RPC Enabled
- **URL:** /xmlrpc.php  
- **Risk:** Enables brute-force amplification (1 request = multiple login attempts) and DDoS attacks  
- **Remediation:** Blocked via .htaccess  
- **Status:** FIXED  

---

### [MEDIUM] M1: Missing Security Headers
- **Missing Headers:**  
  - X-Frame-Options  
  - X-Content-Type-Options  
  - Referrer-Policy  
  - Permissions-Policy  
- **Risk:** Clickjacking, MIME sniffing, and data leakage  
- **Remediation:** Added via .htaccess  
- **Status:** FIXED  

---

### [MEDIUM] M2: HTTP Served Before HTTPS Redirect
- **Issue:** Initial requests served over HTTP before redirect  
- **Risk:** Potential interception (MITM) during initial request  
- **Remediation:** Forced HTTPS via .htaccess  
- **Status:** FIXED  

---

### [MEDIUM] M3: Outdated Plugin – WPBakery Page Builder
- **Installed Version:** 7.0  
- **Latest Version:** 8.7.2  
- **Risk:** Known vulnerabilities in outdated versions  
- **Constraint:** Plugin bundled with theme (no license)  
- **Recommendation:** Purchase license or replace plugin  
- **Status:** CANNOT FIX  

---

### [MEDIUM] M4: Outdated Plugin – Slider Revolution
- **Installed Version:** 6.6.16  
- **Latest Version:** 6.7.35  
- **Risk:** Historical critical vulnerabilities (CVEs)  
- **Constraint:** Plugin bundled with theme (no license)  
- **Recommendation:** Purchase license or replace plugin  
- **Status:** CANNOT FIX  

---

### [MEDIUM] M5: WordPress Version Disclosure
- **Issue:** Version visible in meta tags and scripts  
- **Risk:** Attackers can target version-specific vulnerabilities  
- **Remediation:** Updated WordPress to latest version  
- **Status:** FIXED  

---

### [MEDIUM] M6: WAF in Learning Mode
- **Issue:** Firewall not actively blocking threats  
- **Risk:** Reduced protection against malicious traffic  
- **Remediation:** Switched Wordfence to Enabled mode  
- **Status:** FIXED  

---

### [LOW] L1: readme.html Accessible
- **Risk:** Discloses WordPress version  
- **Remediation:** File removed  
- **Status:** FIXED  

---

### [LOW] L2: PHP Version Disclosure
- **Issue:** x-powered-by header exposed PHP version  
- **Risk:** Targeted exploitation of PHP vulnerabilities  
- **Remediation:** Header removed via .htaccess  
- **Status:** FIXED  

---

### [LOW] L3: VirusTotal Flag
- **Observation:** 1/96 vendors flagged site (Bfore.Ai PreCrime)  
- **Assessment:** Likely false positive  
- **Recommendation:** Re-scan after 30 days  
- **Status:** MONITORING  

---

### [LOW] L4: DISALLOW_FILE_EDIT Not Set
- **Risk:** Admins can edit plugin/theme files directly  
- **Remediation:** Added DISALLOW_FILE_EDIT to wp-config.php  
- **Status:** FIXED  

---

### [LOW] L5: No Two-Factor Authentication
- **Risk:** Account takeover if password compromised  
- **Remediation:** Enabled Wordfence 2FA  
- **Status:** FIXED  
---

## Remediation Summary
- Total findings: 14
- Fixed: 11
- Cannot fix: 2  
- Monitoring: 1
- Risk reduction: Critical attack vectors closed
