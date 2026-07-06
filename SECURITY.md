# Security Policy

## Supported Versions

The following versions of the project currently receive security updates.

| Version | Supported |
| -------- | --------- |
| main | ✅ Yes |
| develop | ✅ Yes |
| older releases | ❌ No |

---

## Reporting a Vulnerability

If you discover a security vulnerability, please **do not open a public GitHub Issue**.

Instead, report it privately to the project maintainers.

### Preferred Contact

- GitHub Security Advisories (recommended)
- Email: security@undangan-digital.com *(replace with your actual email when available)*

Please include as much information as possible:

- Description of the vulnerability
- Steps to reproduce
- Expected behavior
- Actual behavior
- Affected version or commit hash
- Proof of Concept (PoC), if available
- Suggested mitigation (optional)

---

## Response Timeline

We aim to respond according to the following timeline:

| Action | Target Time |
|---------|-------------|
| Initial acknowledgement | Within 48 hours |
| Initial investigation | Within 7 days |
| Status update | Every 7 days |
| Security fix | Depending on severity |

Critical vulnerabilities will be prioritized immediately.

---

## Disclosure Policy

We follow a **responsible disclosure** process.

1. The vulnerability is reported privately.
2. The maintainers verify and reproduce the issue.
3. A fix is developed and tested.
4. The fix is released.
5. A public security advisory is published if necessary.

Please allow us reasonable time to investigate before publicly disclosing any vulnerability.

---

## Scope

This security policy covers all official components of the project, including:

- Laravel Backend API
- Next.js Frontend
- PostgreSQL Database
- Redis
- Authentication (JWT)
- Payment Gateway Integration
- File Upload Service
- Admin Dashboard
- Builder Engine
- Deployment Infrastructure
- GitHub Actions CI/CD

Third-party services (such as Midtrans, Cloudflare, AWS S3, Mailgun, Google OAuth, etc.) should be reported directly to their respective vendors if the vulnerability originates from their infrastructure.

---

## Security Best Practices

Contributors are expected to follow these practices:

- Never commit secrets, API keys, or passwords.
- Never commit `.env` files.
- Enable Two-Factor Authentication (2FA) on GitHub.
- Keep dependencies updated.
- Run static analysis before submitting Pull Requests.
- Validate all user input.
- Follow OWASP Top 10 recommendations.
- Use parameterized database queries.
- Apply the Principle of Least Privilege.

---

## Security Tools

This project may use the following security tools:

- GitHub Dependabot
- GitHub Secret Scanning
- CodeQL Analysis
- PHPStan
- Laravel Pint
- Composer Audit
- npm audit
- Trivy (Container Scanning)
- Snyk (optional)

---

## Supported Authentication

The platform uses:

- JWT Authentication
- Refresh Token Rotation
- OAuth 2.0 (Google)
- Role-Based Access Control (RBAC)
- Laravel Policies & Gates

---

## Infrastructure Security

Production deployments should implement:

- HTTPS (TLS 1.2+)
- HSTS
- Secure Cookies
- CSRF Protection
- Rate Limiting
- Redis-backed Queue
- PostgreSQL Row-Level Security (RLS)
- Cloudflare WAF
- Daily Database Backup
- Continuous Monitoring
- Centralized Logging

---

## Acknowledgements

We appreciate everyone who helps improve the security of this project through responsible vulnerability reporting.

Thank you for helping keep **Digital Invitation Platform** secure.
