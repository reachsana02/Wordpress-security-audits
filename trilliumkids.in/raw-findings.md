# Raw Findings – Security Audit
## trilliumkids.in

**Audit Date:** 23 March 2026  
**Auditor:** Sana Fathima  
**Site Type:** Children's Education Website  

---

## Tools Used
- WPScan 3.8.28  
- Qualys SSL Labs  
- SecurityHeaders.com (Snyk)  
- VirusTotal  
- MXToolbox  
- Google Safe Browsing  
- Manual Browser Testing  

---

## Findings

### HIGH Severity

#### H1 – User Enumeration via Author Pages
- **Endpoint:** /?author=1, /?author=2  
- **Exposed:** reacharmaanahmed, sadiq  
- **Risk:** Enables username harvesting for brute-force attacks  
- **Status:** FIXED  

---

#### H2 – REST API User Data Exposure
- **Endpoint:** /wp-json/wp/v2/users  
- **Exposed:** Usernames, IDs, avatars  
- **Risk:** Unauthenticated user enumeration  
- **Status:** FIXED  

---

#### H3 – XML-RPC Enabled
- **Endpoint:** /xmlrpc.php  
- **Risk:** Brute-force amplification and DDoS vector  
- **Status:** FIXED  

---

### MEDIUM Severity

#### M1 – Missing Security Headers
- **Missing:** X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy  
- **Risk:** Clickjacking, MIME sniffing, data exposure  
- **Status:** FIXED  

---

#### M2 – HTTP Before HTTPS Redirect
- **Risk:** MITM attack possibility during initial request  
- **Status:** FIXED  

---

#### M3 – Outdated Plugin: WPBakery Page Builder
- **Installed:** v7.0  
- **Latest:** v8.7.2  
- **Risk:** Known vulnerabilities  
- **Status:** CANNOT FIX (license constraint)  

---

#### M4 – Outdated Plugin: Slider Revolution
- **Installed:** v6.6.16  
- **Latest:** v6.7.35  
- **Risk:** Historically critical vulnerabilities  
- **Status:** CANNOT FIX (license constraint)  

---

#### M5 – WordPress Version Disclosure
- **Exposed:** Version 6.8.3  
- **Risk:** Targeted attacks using known CVEs  
- **Status:** FIXED  

---

#### M6 – WAF in Learning Mode
- **Risk:** Firewall not enforcing protections  
- **Status:** FIXED  

---

### LOW Severity

#### L1 – readme.html Accessible
- **Risk:** Version disclosure  
- **Status:** FIXED  

---

#### L2 – PHP Version Disclosure
- **Header:** x-powered-by  
- **Risk:** Targeted exploitation  
- **Status:** FIXED  

---

#### L3 – VirusTotal Flag
- **Detection:** 1/96 vendors  
- **Assessment:** Likely false positive  
- **Status:** MONITORING  

---

#### L4 – DISALLOW_FILE_EDIT Missing
- **Risk:** Post-compromise file editing  
- **Status:** FIXED  

---

#### L5 – No Two-Factor Authentication
- **Risk:** Account takeover  
- **Status:** FIXED  

---

## Passed Checks
- SSL Grade: A  
- Google Safe Browsing: Clean  
- MXToolbox Blacklists: Clean  
- No backup/config files exposed  
- Contact Form 7: Up to date  
- MonsterInsights: Up to date  

---

## Summary
- **Total Findings:** 14  
- **Fixed:** 11  
- **Cannot Fix:** 2  
- **Monitoring:** 1  

**Risk Reduction:** HIGH → LOW-MEDIUM  

Critical attack vectors (XML-RPC, user enumeration, REST API exposure) were successfully mitigated.

---

## Auditor Notes
This audit was conducted on a live production website with full authorization.

The website handles parent contact submissions, increasing the importance of maintaining strong security controls.

WPBakery and Slider Revolution remain outdated due to licensing limitations. The client has been advised to purchase licenses or migrate to alternative plugins.

The Wordfence firewall successfully blocked automated scanning attempts, indicating effective perimeter protection.
