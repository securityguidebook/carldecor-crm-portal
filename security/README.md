# 🔒 Carl Decor Handyman - Security Hardening Documentation

[![Security Headers](https://img.shields.io/badge/securityheaders-A%2B-brightgreen)](https://securityheaders.com/?q=https%3A//securityguidebook.github.io/carldecor-crm-portal&hideOnInvalid=1)
[![GitHub Pages](https://img.shields.io/badge/Live%20Site-Ready-brightgreen)](https://securityguidebook.github.io/carldecor-crm-portal)

## Live Production Site
[**https://securityguidebook.github.io/carldecor-crm-portal**](https://securityguidebook.github.io/carldecor-crm-portal)

**Real SMB client portal** generating leads for family handyman business.

## Threat Model & Mitigations

| Threat | Impact | Mitigation | Status |
|--------|--------|------------|--------|
| **Spam bots** | Flood business email | **hCaptcha** + **Rate limiting (3/hr)** | ✅ LIVE |
| **XSS attacks** | Malicious scripts | **CSP headers** (`default-src 'self'`) | ✅ A+ scan |
| **Injection** | Form exploits | **Input sanitization** (regex + length limits) | ✅ Client‑side |
| **Clickjacking** | UI redress | **X‑Frame‑Options: DENY** | ✅ Enforced |
| **MIME sniffing** | Content exploits | **X‑Content-Type-Options: nosniff** | ✅ Enabled |

## Security Scan Results
