# PortSwigger Web Security Academy Lab Report  
SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

**Report ID:** PS-LAB-001  
**Author:** Abhiram (Abhi)  
**Date:** January 24, 2026  
**Lab Level:** Apprentice  
**Lab Title:** SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

## Executive Summary

- **Vulnerability Type:** SQL Injection (Boolean-based / OR condition bypass)  
- **Severity:** High (CVSS 3.1 Score: 8.6 – AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N)

**Description:** A SQL injection vulnerability was identified in the `category` parameter of the `/filter` endpoint. The backend query is like `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`. The payload `' OR 1=1 --` bypasses the `released = 1` filter, displaying hidden/unreleased products.

**Impact:** Unauthorized exposure of restricted data (e.g., unreleased products). In production, this could lead to database enumeration, credential theft, or privilege escalation.

**Status:** Exploited only in the controlled lab; no real-world impact. Educational purposes only.

## Environment and Tools Used

- **Target:** Simulated e-commerce site from PortSwigger Web Security Academy (e.g., `https://*.web-security-academy.net`)  
- **Browser:** Google Chrome (Version 120.0 or similar)  
- **Tools:**  
  - Burp Suite Community Edition (Version 2023.12 or similar) – request interception/modification  
- **Operating System:** Windows 11  
- **Test Date/Time:** January 24, 2026, ~04:17 PM IST

## Methodology

1. Accessed lab via "Access the lab" in PortSwigger Academy.  
2. Added base URL to Burp Suite target scope.  
3. Enabled Intercept in Burp Proxy, selected "Gifts" category to capture request.  
4. Modified `category` parameter:  
   - `category='` → triggered SQL error.  
   - `category=' OR 1=1 --` → displayed all products (including hidden).  
5. Analyzed in Burp Proxy HTTP history / Target tab.

## Detailed Findings

**Vulnerable Endpoint:** `GET /filter?category=...`

**Original Request:**

```http
GET /filter?category=gifts HTTP/1.1
Host: *.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
Connection: close
...