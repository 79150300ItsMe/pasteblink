# Security Guidelines for pasteblink

This document outlines security best practices tailored to the `pasteblink` starter template. It maps core security principles to each component of the codebase and provides actionable recommendations for safeguarding your application by design.

---

## 1. Secure Authentication & Access Control

### 1.1 Strengthen Password Handling
- Leverage Better Auth’s built-in hashing mechanism (e.g., Argon2 or bcrypt) with unique per-user salts.  
- Enforce a strong password policy: minimum length (e.g., 12 characters), mix of letters, numbers, and symbols.  
- Consider integrating password rotation or reuse prevention for higher-risk deployments.

### 1.2 Session Management
- Configure secure, HttpOnly, `SameSite=Strict` cookies for session tokens.  
- Enforce both idle and absolute session timeouts (e.g., 15 min idle, 8 hours absolute).  
- Protect against session fixation by regenerating session IDs upon login.

### 1.3 JWT / Token Security
- If Better Auth uses JWTs, validate the `alg` header (no “none”), expiration (`exp`), and audience/issuer claims.  
- Rotate signing keys periodically and store them securely (e.g., AWS KMS, HashiCorp Vault).

### 1.4 Role-Based Access Control (RBAC)
- Define clear user roles (e.g., `user`, `admin`) in your Drizzle ORM schema.  
- Enforce authorization checks server-side on every API route and server component.  
- Use middleware in Next.js (`middleware.ts`) to guard protected routes (e.g., `/dashboard`).

### 1.5 Multi-Factor Authentication (MFA)
- Offer optional MFA (TOTP or SMS) for elevated-privilege accounts or sensitive operations.

---

## 2. Input Handling & Processing

### 2.1 Server-Side Validation
- Adopt a schema validation library (e.g., Zod) for all request bodies and query parameters in API routes.  
- Mirror client-side validation with identical server-side schemas to prevent bypass.

### 2.2 Prevent Injection Attacks
- Use Drizzle ORM’s parameterized queries—avoid interpolating raw SQL or user input.  
- Sanitize any dynamic values before passing them to third-party services or the shell.

### 2.3 Mitigate XSS and Template Injection
- Encode all user-supplied data rendered in React components.  
- Use the built-in escaping in JSX and avoid dangerouslySetInnerHTML unless sanitized.
- Apply a strict Content Security Policy (CSP) via HTTP headers.

### 2.4 Secure File Uploads (Future Feature)
- If enabling file uploads, validate file type, size, and scan for malware.  
- Store files outside the webroot or in cloud storage with ACL restrictions.

---

## 3. Data Protection & Privacy

### 3.1 Encrypt Data In Transit
- Enforce HTTPS (TLS 1.2+) for all front-end and back-end communication.  
- Redirect HTTP to HTTPS with HSTS headers (`Strict-Transport-Security`).

### 3.2 Encrypt Data At Rest
- Enable PostgreSQL’s data encryption or rely on encrypted volumes in Docker.  
- Ensure backups are encrypted and access-controlled.

### 3.3 Secure Secrets Management
- Avoid hard-coding connection strings or API keys in source.  
- Use environment variables injected securely via a secrets manager (e.g., AWS Secrets Manager, Vault).

### 3.4 Prevent Information Leakage
- Sanitize error messages: return generic messages to clients, log detailed errors locally.  
- Mask or redact PII in logs and responses.

---

## 4. API & Service Security

### 4.1 Enforce HTTPS & CORS
- Configure Next.js to only accept secure connections.  
- Restrict CORS to allowed origins (e.g., your production domain).

### 4.2 Rate Limiting & Throttling
- Implement rate limiting on authentication endpoints (`/api/auth`) to mitigate brute-force attacks.  
- Consider an in-memory store (e.g., Redis) or API gateway rules.

### 4.3 Proper HTTP Methods & Versioning
- Use POST for state-changing actions (sign-in, sign-up), GET for safe reads.  
- Version your API routes (e.g., `/api/v1/auth/…`) to enable controlled evolution.

---

## 5. Web Application Security Hygiene

### 5.1 CSRF Protection
- Use synchronized anti-CSRF tokens on all state-changing forms and API calls.  
- Leverage Next.js middleware or libraries like `next-csrf`.

### 5.2 Security Headers
- Content-Security-Policy: restrict scripts, styles, and media to trusted sources.  
- X-Frame-Options: `DENY` or `SAMEORIGIN` to prevent clickjacking.  
- X-Content-Type-Options: `nosniff` to block MIME sniffing.  
- Referrer-Policy: `no-referrer-when-downgrade` or stricter.

### 5.3 Secure Cookies
- Mark session cookies `Secure`, `HttpOnly`, and `SameSite=Strict`.

### 5.4 Subresource Integrity (SRI)
- If loading third-party scripts or stylesheets from CDNs, include SRI hashes.

---

## 6. Infrastructure & Configuration Management

### 6.1 Docker & Container Security
- Run containers with a non-root user.  
- Limit exposed ports to only those required (e.g., 443).  
- Scan Docker images for vulnerabilities (e.g., Trivy, Clair).

### 6.2 Secure Defaults & Hardening
- Disable debug logs and Next.js `dev` mode in production.  
- Remove or secure Next.js telemetry and diagnostic endpoints.

### 6.3 TLS/SSL Configuration
- Use strong cipher suites.  
- Disable TLS 1.0/1.1 and weak ciphers in your load balancer or web server.

---

## 7. Dependency Management

- Maintain a lockfile (`package-lock.json`) and review dependency updates regularly.  
- Integrate an SCA tool (e.g., Dependabot, Snyk) to detect and remediate vulnerable packages.  
- Audit third-party libraries (Next.js, shadcn/ui, Drizzle ORM, Better Auth) for active maintenance and security advisories.

---

Adhering to these guidelines will help you build on the `pasteblink` template with a strong security posture from day one. Always treat security as an ongoing process: continuously review, test, and update your application as threats evolve.
