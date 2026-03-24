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
SITE: trilliumkids.in
TYPE: Children's education website
AUDIT DATE: 23 March 2026
AUDITOR: Sana Fathima
TOOLS USED: WPScan 3.8.28, SSLLabs, SecurityHeaders.com, 
            VirusTotal, MXToolbox, Google Safe Browsing, 
            Manual browser testing

════════════════════════════════════
FINDINGS
════════════════════════════════════

[HIGH] H1 : User Enumeration via Author Pages
- URL: /?author=1 and /?author=2
- Usernames exposed: reacharmaanahmed, sadiq
- Risk: Attacker can obtain valid usernames for brute force attacks
- Status: FIXED - redirect added via .htaccess

[HIGH] H2 : REST API Exposes User Data Unauthenticated  
- URL: /wp-json/wp/v2/users
- Data exposed: usernames, IDs, avatar URLs for 2 users
- Risk: Direct username harvesting without any authentication
- Status: FIXED - endpoint removed via WPCode PHP snippet

[HIGH] H3 : XML-RPC Enabled
- URL: /xmlrpc.php accessible and responding
- Risk: Enables brute force amplification attacks 
  (1 request = 1000 password attempts), DDoS amplification
- Status: FIXED - blocked via .htaccess

[MEDIUM] M1 : Security Headers Missing
- Missing: X-Frame-Options, X-Content-Type-Options, 
  Referrer-Policy, Permissions-Policy
- Risk: Clickjacking, MIME sniffing, information leakage
- Status: FIXED - all headers added via .htaccess

[MEDIUM] M2 : HTTP Served Before HTTPS Redirect
- Site was serving HTTP before redirecting to HTTPS
- Risk: Traffic interception on initial request
- Status: FIXED - HTTPS forced via .htaccess

[MEDIUM] M3 : Outdated Plugin: WPBakery Page Builder
- Installed: v7.0 | Latest: v8.7.2
- Risk: Known vulnerabilities in older versions
- Status: CANNOT FIX - bundled with theme, no direct license
- Recommendation: Purchase direct license or replace plugin

[MEDIUM] M4 : Outdated Plugin: Slider Revolution
- Installed: v6.6.16 | Latest: v6.7.35
- Risk: Slider Revolution has history of critical CVEs
- Status: CANNOT FIX - bundled with theme, no direct license
- Recommendation: Purchase direct license or replace plugin

[MEDIUM] M5 — WordPress Version Publicly Disclosed
- Version 6.8.3 visible in meta tags and script URLs
- Risk: Attackers can target known CVEs for this version
- Status: FIXED - Updates to version 6.9.4

[LOW] L1 : readme.html Accessible
- Disclosed WordPress version publicly
- Status: FIXED - file deleted from server

[LOW] L2 : PHP Version Disclosed in Headers
- x-powered-by: PHP/8.2.29 visible in response headers
- Status: FIXED - header unset via .htaccess

[LOW] L3 : VirusTotal Flag
- 1/96 vendors flagged as malicious (Bfore.Ai PreCrime)
- Assessment: Likely false positive - 95/96 vendors clean
- Status: MONITORING - recommend re-scan in 30 days

════════════════════════════════════
PASSED CHECKS
════════════════════════════════════
- SSL Grade: A (all 4 servers)
- Google Safe Browsing: Clean
- Domain Blacklists: Clean (MXToolbox)
- No config backup files exposed
- Contact Form 7: Up to date
- MonsterInsights: Up to date

════════════════════════════════════
REMEDIATION SUMMARY
════════════════════════════════════
Total Findings: 10
Fixed: 7
Cannot Fix (licensing): 2
Monitoring: 1

Risk Reduction: HIGH → the 3 most critical 
attack vectors (XML-RPC, user enumeration, 
REST API exposure) have all been closed.

════════════════════════════════════
ADDITIONAL FINDINGS
════════════════════════════════════

[MEDIUM] M6 : Wordfence Firewall in Learning Mode
- Firewall was installed but set to Learning Mode
- Risk: Firewall not actively blocking threats
- Status: FIXED - switched to Enabled mode

[LOW] L4 : DISALLOW_FILE_EDIT Not Set
- wp-config.php did not contain DISALLOW_FILE_EDIT
- Risk: Admin users can edit theme/plugin files 
  directly, common post-compromise technique
- Status: FIXED - added to wp-config.php

[LOW] L5 : No 2FA on Admin Account
- Admin account had no two-factor authentication
- Risk: Account takeover if password is compromised
- Status: FIXED - Wordfence 2FA activated

════════════════════════════════════
TOOLS & VERSIONS
════════════════════════════════════
WPScan: 3.8.28
Wordfence: Free tier
SSLLabs: Qualys SSL Server Test
SecurityHeaders.com: by Snyk
VirusTotal: URL scan
MXToolbox: Blacklist check
Google Safe Browsing: Transparency Report

════════════════════════════════════
AUDITOR NOTES
════════════════════════════════════
This audit was conducted on a live production 
website. All fixes were applied with owner 
permission. The site serves a children's education 
institution and handles parent contact form 
submissions - making security especially important.

WPBakery and Slider Revolution could not be updated 
due to licensing constraints (bundled with premium 
theme). Client has been advised to purchase direct 
licenses. No active exploits were confirmed against 
the installed versions during this assessment period.

The Wordfence WAF blocked WPScan during initial 
scanning - this is a positive indicator that the 
server-level firewall is functioning correctly.


---

## Remediation Summary
- Total findings: 14
- Fixed: 11
- Cannot fix: 2  
- Monitoring: 1
- Risk reduction: Critical attack vectors closed
