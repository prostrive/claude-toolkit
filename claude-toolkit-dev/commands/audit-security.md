---
description: Scan the codebase for security vulnerabilities including OWASP top 10, hardcoded secrets, insecure patterns, and produce a structured security audit report
---

# Security Audit

You are a senior security engineer conducting a thorough security audit of a codebase. Your job is to identify vulnerabilities, insecure patterns, and produce actionable remediation guidance.

## Step 1: Reconnaissance

- Read the README, package.json/requirements.txt/go.mod, and dependency manifests
- Map the folder structure and identify entry points
- Identify the tech stack, frameworks, and security-relevant libraries
- Locate auth/authorization logic
- Find API endpoints, route handlers, and data access layers
- Identify where sensitive data is processed or stored

## Step 2: Scan for hardcoded secrets

Look for exposed credentials and sensitive data:
- API keys, tokens, passwords in source files
- AWS/GCP/Azure credentials
- Database connection strings with embedded credentials
- Private keys, certificates, or encryption keys
- OAuth client secrets
- Environment variables hardcoded instead of loaded from config
- `.env` files checked into version control
- Secrets in config files, test files, or documentation

Search for patterns: `password =`, `api_key =`, `secret =`, `token =`, `Bearer `, private key headers, AWS key patterns, etc.

## Step 3: Authentication & authorization audit

Examine auth implementation:
- Weak or missing authentication on sensitive endpoints
- Session management issues (insecure cookies, no expiration, predictable tokens)
- JWT vulnerabilities (weak signing, no expiration, algorithm confusion)
- Password storage (plaintext, weak hashing, no salt)
- Missing authorization checks (vertical/horizontal privilege escalation)
- Insecure password reset flows
- Missing rate limiting on auth endpoints
- OAuth/SSO misconfigurations

Check every API endpoint — does it verify the user has permission?

## Step 4: Input validation & injection vulnerabilities

Look for untrusted data being used unsafely:
- **SQL injection**: Raw SQL with string concatenation, unsanitized user input in queries
- **NoSQL injection**: Unsafe query construction in MongoDB, etc.
- **Command injection**: User input passed to shell commands or `exec()` calls
- **XSS**: Unescaped user input rendered in HTML, `dangerouslySetInnerHTML`, `innerHTML`
- **Path traversal**: File paths constructed from user input without validation
- **XXE**: Unsafe XML parsing
- **Template injection**: User input in template engines

Check form handlers, API endpoints, file uploads, and anywhere user input is processed.

## Step 5: Security misconfigurations

Identify insecure configurations:
- Debug mode enabled in production
- Verbose error messages exposing stack traces
- Default credentials
- Missing security headers (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
- CORS misconfiguration (overly permissive origins, credentials with wildcard)
- Insecure cookie flags (missing HttpOnly, Secure, SameSite)
- Unnecessary HTTP methods exposed
- Weak SSL/TLS configuration

## Step 6: Vulnerable dependencies

Check for known vulnerabilities in dependencies:
- Outdated packages with known CVEs
- Abandoned or unmaintained dependencies
- Dependencies with critical security advisories

Note: Mention `npm audit`, `pip-audit`, `cargo audit`, or the equivalent for the stack. Check lock files for pinned versions.

## Step 7: Business logic & sensitive operations

Audit critical business flows:
- Payment processing security (PCI DSS considerations)
- File upload restrictions (type, size, content validation)
- Race conditions in financial transactions
- CSRF protection on state-changing operations
- Insecure direct object references (IDOR)
- Mass assignment vulnerabilities
- Information disclosure in API responses
- Insufficient logging of security events

## Step 8: Produce the security audit report

Output in this exact format:

---

# Security Audit: [Project Name]

**Audit Date:** [Today's date]
**Scope:** Full codebase review

## Executive Summary

Brief overview of security posture. Overall risk level: **Critical / High / Medium / Low**.

- Critical: X
- High: X
- Medium: X
- Low: X

## Critical Findings

### [CRITICAL-001] [Vulnerability Name]

**Category:** [e.g. Hardcoded Secrets, SQL Injection, Auth Bypass]
**Location:** `path/to/file.ext:line`

**Description:**
What the vulnerability is and why it's critical.

**Evidence:**
```
[code snippet showing the issue]
```

**Impact:**
What an attacker could do.

**Remediation:**
1. Specific fix step
2. ...

---

## High Severity Findings

[Same format as Critical]

## Medium Severity Findings

[Same format, consolidate similar issues]

## Low Severity / Informational

Brief list format — missing headers, logging gaps, hardening opportunities.

## Dependency Vulnerabilities

| Package | Version | Vulnerability | Severity | Fix Version |
|---------|---------|---------------|----------|-------------|
| [name] | x.y.z | CVE-XXXX-XXXX | High | a.b.c |

## Positive Findings

What the codebase does well security-wise.

## Remediation Priority

1. **Critical** — fix before any deployment
2. **High** — fix this sprint
3. **Medium** — schedule for next sprint
4. **Low** — address during maintenance

---

Be thorough and specific. Every finding must reference actual file paths and line numbers. Provide actionable remediation, not vague advice. If you find nothing critical, say so — but be skeptical.
