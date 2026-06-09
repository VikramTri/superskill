---
name: security-hardener
description: Dedicated skill to audit codebases for security vulnerabilities, enforce input validation/sanitization, configure CSP and secure HTTP headers, manage environment secrets, and perform dependency audits.
---
# Security Hardener Specialist

This skill directs the agent on how to safeguard application data, protect against common security attack vectors (OWASP Top 10), and secure build and deployment pipelines.

## Security Practices & Mitigations

### 1. Input Validation & Sanitization (XSS & Injection Protection)
- **Sanitize HTML:** Never render unescaped user-controlled text directly using elements like `dangerouslySetInnerHTML` in React or raw HTML interpolation in vanilla JavaScript. Use libraries like `dompurify` to strip malicious scripts.
  ```javascript
  import DOMPurify from 'dompurify';
  const cleanHtml = DOMPurify.sanitize(userInputHtml);
  ```
- **Prepared Statements:** Prevent SQL injection by always utilizing parameterized queries or ORM models rather than manual string concatenation in SQL queries.
- **Strict Schema Validation:** Validate incoming request payloads (headers, query parameters, body) using schema validation libraries like `zod`, `yup`, or `joi`.

### 2. HTTP Headers & CORS Configurations
- **Security Headers:** Configure secure headers using middleware such as `helmet` for Node/Express.
  - Set `X-Frame-Options: DENY` to prevent clickjacking.
  - Set `X-Content-Type-Options: nosniff` to prevent MIME-type sniffing.
- **Content Security Policy (CSP):** Enforce a strict CSP header to control which sources are allowed to execute scripts, styles, and load assets.
- **Cross-Origin Resource Sharing (CORS):** Avoid wildcards in CORS (`Access-Control-Allow-Origin: *`) for sensitive authentication routes. Explicitly define allowed origins.

### 3. Session & Token Management
- **HttpOnly Cookies:** Store JWTs or session identifiers in `HttpOnly`, `Secure`, and `SameSite=Strict` (or `Lax`) cookies to protect them from cross-site scripting (XSS) retrieval.
- **Token Expiry & Revocation:** Ensure tokens have short lifespans and implement robust token refresh/revocation schemes.

### 4. Secrets Management
- **Never Commit Secrets:** Ensure passwords, private keys, database credentials, and OAuth secrets are never hard-coded or committed to git.
- **Environment Variables:** Leverage `.env` files locally and secure environment stores in CI/CD (such as GitHub Actions Secrets). Add `.env` to `.gitignore`.
- **Validation:** Validate that all required environment variables are present on application startup to prevent silent configuration failures.

---

## Auditing & Verification Workflow

1. **Dependency Audit:** Run `npm audit` or equivalent dependency scanning commands using the `run_command` tool to find packages with known vulnerabilities (CVEs).
2. **Static Code Analysis (SAST):** Run linters (like ESLint with plugins such as `eslint-plugin-security` or `eslint-plugin-no-unsanitized`) to spot insecure coding patterns.
3. **Secret Scanning:** Scan the commit history using tools like `trufflehog` or `gitleaks` (if available) to verify that no historical credentials have leaked into git.
4. **Manual Code Review:** Inspect data ingestion flows, session creation steps, and system command executions (`child_process.exec`, etc.) to ensure no unsanitized user inputs are passed directly to OS sub-shells.
