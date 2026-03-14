# 🔒 Carl Decor Handyman - Security Hardening Documentation

[![Security Headers](https://img.shields.io/badge/securityheaders-A%2B-brightgreen)](https://securityheaders.com/?q=https%3A//securityguidebook.github.io/carldecor-crm-portal&hideOnInvalid=1)
[![GitHub Pages](https://img.shields.io/badge/Live%20Site-Ready-brightgreen)](https://securityguidebook.github.io/carldecor-crm-portal)

This repo secures a real small‑business lead portal for a family handyman business in Auckland, NZ.  
Live site: https://securityguidebook.github.io/carldecor-crm-portal

---

## 1. Project overview

**Goal:** Build a simple CRM‑style contact portal for a handyman business, then harden it using practical web security controls suitable for a small business.

**Tech stack:**
- Static site on GitHub Pages (HTML/CSS/JS)
- EmailJS for form delivery (no backend server)
- Client‑side security controls and HTTP security headers

---

## 2. Threat model (small service business)

| Asset                     | Threat                        | Impact                         |
|---------------------------|-------------------------------|--------------------------------|
| Contact form / inbox      | Spam bots, scripted abuse     | Inbox flooding, lost real jobs |
| Form fields               | XSS / injection payloads      | Defacement, data theft         |
| Users visiting the site   | Clickjacking, mixed content   | Tricked clicks, unsafe loads   |

---

## 3. Controls implemented

### 3.1 Input validation & sanitization

- Enforce length limits on name and message (2–50 chars, 10–1000 chars).
- Validate email **or** phone with a strict regular expression.
- Trim whitespace before processing.

```js
const name = document.getElementById('name').value.trim();
const contact = document.getElementById('contact').value.trim();
const message = document.getElementById('message').value.trim();

if (name.length < 2 || name.length > 50) { alert('Name: 2–50 chars'); return; }
if (!contact.match(/^[\w.%+-]+@[\w.-]+\.[A-Za-z]{2,}$|^[0-9\s\-\+\(\)]{10,15}$/)) {
  alert('Valid email OR phone required'); return;
}
if (message.length < 10 || message.length > 1000) { alert('Message: 10–1000 chars'); return; }
```
Why: Reduces injection and XSS risk and makes automated spam harder.

### 3.2 Rate limiting (3 submissions per hour per browser)
- Track submissions in localStorage.
- Block further submissions for 1 hour after 3 successful requests.

```js
let submitCount = parseInt(localStorage.getItem('submitCount') || '0');
let lastSubmit  = parseInt(localStorage.getItem('lastSubmit')  || '0');

if (Date.now() - lastSubmit < 3600000 && submitCount >= 3) {
  alert('Rate limit: max 3 requests per hour.');
  return;
}

// on successful send:
localStorage.setItem('submitCount', ++submitCount);
localStorage.setItem('lastSubmit', Date.now());
```
Why: Limits automated abuse and form flooding without a backend.

### 3.3 EmailJS integration (no server‑side secrets)
- Uses EmailJS public key and service/template IDs only.
- No API secrets or credentials are stored in the repo.
- Emails go directly to the family business inbox with structured fields.

```js
emailjs.init('LkVmYK7zEZV-dWSUQ');

emailjs.send('service_jap8g87', 'template_hmzbsuy', {
  name, contact,
  service: document.getElementById('service').value,
  message,
  date: document.getElementById('date').value
});
Why: Avoids hosting a vulnerable backend while still delivering real leads.
```

### 3.4 Security headers (defence in depth)
Configured to improve the score on tools like securityheaders.com and protect visitors:
- Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.jsdelivr.net; style-src 'self' 'unsafe-inline'
- X-Frame-Options: DENY
- X-Content-Type-Options: nosniff
- Referrer-Policy: strict-origin-when-cross-origin
- Permissions-Policy: geolocation=(), microphone=(), camera=()

Why:
- CSP helps block XSS and unauthorized scripts.
- X‑Frame‑Options stops clickjacking.
- X‑Content‑Type‑Options prevents MIME sniffing attacks.
- Referrer‑Policy and Permissions‑Policy reduce data and feature abuse.

## 4. User‑flow: secure job request
1. User opens the contact form over HTTPS on GitHub Pages.
2. Enters name, contact details, selects service, and describes the job.
3. Form validation runs (length checks, regex checks).
4. (Optional) CAPTCHA / anti‑bot check is performed.
5. Rate limiter ensures no more than 3 successful submissions per hour.
6. EmailJS sends a structured job request email to the business inbox.
7. Any errors are shown in a friendly message to the user.

## 5. Results & next steps
What this project demonstrates:
- Ability to design and build a small‑business web portal from scratch.
- Practical application of web security controls (validation, rate limiting, headers).
- Use of third‑party services (EmailJS) safely, without exposing secrets.
- Clear documentation aimed at both technical reviewers and non‑technical owners.

Planned improvements:
- Move to a custom domain fronted by Cloudflare to enforce all headers (including HSTS) at the edge.
- Add basic logging/alerting for unusual submission patterns.
- Expand documentation with a privacy policy and data‑retention guidelines.

## 6. How to reuse this
Other small businesses can fork this repo and:
- Change branding and services in index.html.
- Update the EmailJS service and template IDs.
- Keep the same validation, rate limiting, and security header patterns as a starter security baseline.
