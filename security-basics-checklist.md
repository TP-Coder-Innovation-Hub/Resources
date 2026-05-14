# Security Basics Checklist

Quick reference for common security concerns when building web applications.

## Input Validation

- [ ] Validate all user input on the server side (never trust client-side validation alone)
- [ ] Use allowlists (whitelist valid values) over blocklists
- [ ] Validate type, length, format, and range
- [ ] Sanitize input before storing or displaying (prevent XSS)

## Authentication

- [ ] Never store passwords in plain text — use bcrypt or argon2
- [ ] Use established auth libraries/OAuth providers instead of rolling your own
- [ ] Implement password strength requirements
- [ ] Use multi-factor authentication (MFA) for sensitive operations
- [ ] Set reasonable session expiry times
- [ ] Invalidate tokens on logout

## Authorization

- [ ] Check permissions on every request, not just in the UI
- [ ] Use the principle of least privilege — give users only the access they need
- [ ] Never expose internal IDs if users can guess other users' resources (use UUIDs or check ownership)

## OWASP Top 10 (Quick Summary)

| # | Risk | Quick fix |
|---|---|---|
| A01 | Broken Access Control | Check authorization on every endpoint |
| A02 | Cryptographic Failures | Use HTTPS, encrypt sensitive data at rest |
| A03 | Injection (SQL, XSS) | Use parameterized queries, escape output |
| A04 | Insecure Design | Threat model during design phase |
| A05 | Security Misconfiguration | Disable default accounts, hide error details |
| A06 | Vulnerable Components | Keep dependencies updated |
| A07 | Auth Failures | Use strong password hashing, rate limit logins |
| A08 | Data Integrity Failures | Validate deserialized data, check file integrity |
| A09 | Logging & Monitoring Gaps | Log auth events, monitor for anomalies |
| A10 | SSRF | Validate and restrict outbound requests |

## SQL Injection Prevention

**Never do this:**
```sql
SELECT * FROM users WHERE email = '${email}'
```

**Always do this (parameterized):**
```sql
SELECT * FROM users WHERE email = ?
```

Or use an ORM/query builder that parameterizes automatically.

## XSS (Cross-Site Scripting) Prevention

- Escape all user-generated content before rendering in HTML
- Use Content Security Policy (CSP) headers
- Use `textContent` instead of `innerHTML` when possible
- Set cookies with `HttpOnly` and `Secure` flags

## Secrets Management

- [ ] Never commit secrets (API keys, passwords, tokens) to git
- [ ] Use `.env` files for local development (add `.env` to `.gitignore`)
- [ ] Use environment variables in production
- [ ] Rotate secrets periodically
- [ ] Use a secrets manager (AWS Secrets Manager, HashiCorp Vault) for production

## API Security

- [ ] Use HTTPS everywhere
- [ ] Rate limit your APIs to prevent abuse
- [ ] Validate `Content-Type` headers
- [ ] Return generic error messages (don't leak stack traces or internal details)
- [ ] Use CORS headers intentionally, not `*`

## Quick Pre-Launch Security Checklist

1. No secrets in code or git history
2. All endpoints have authentication where needed
3. All user input is validated and sanitized
4. HTTPS is enforced
5. Error pages don't expose internal details
6. Dependencies are up to date
7. Rate limiting is in place
8. Logging covers auth events (logins, failures, permission changes)
