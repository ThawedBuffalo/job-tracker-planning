# Platform Requirements: job-tracker-planning

> Technical Boundaries & Platform Decisions

**Created:** 2026-03-26
**Author:** JC
**Related:** [Product Brief](./project-brief.md)

---

## Technology Stack

### Core Platform

**CMS/Framework:** Custom application stack (no CMS)
**Approach:** Single VPS deployment with Docker Compose

A single VPS setup is selected to optimize delivery speed for a solo engineer over a 1-month timeline. The platform runs a Flutter Web frontend, Spring Boot API backend, and PostgreSQL database behind Nginx with TLS.

### Key Technologies

| Layer | Technology | Rationale |
|-------|------------|-----------|
| **Frontend** | Flutter Web | Reuse Flutter skills/code patterns and deliver responsive web + mobile web from one codebase |
| **Styling** | Flutter Material + custom theme system | Fast, consistent UI delivery with maintainable design tokens |
| **Backend** | Spring Boot (Java) | Strong API ergonomics, mature ecosystem, reliable auth/email integration patterns |
| **Database** | PostgreSQL | Relational model fits status workflows, audit history, and reporting queries |
| **Hosting** | Single VPS + Docker Compose + Nginx | Lowest operational complexity for v1 while keeping full control |

---

## Plugin/Package Stack

| Package/Component | Purpose | Status |
|-------------------|---------|--------|
| Spring Security | Authentication and authorization | Required |
| JWT library (e.g., `jjwt`) | Token issuance and validation | Required |
| Java Mail sender | SMTP email notifications | Required |
| Jsoup (or equivalent parser) | URL metadata extraction for job intake | Required |
| Flyway/Liquibase | Database schema migrations | Required |

---

## Integrations

### Required Integrations

- **SMTP provider:** Send stage-change and reminder emails (provider-agnostic SMTP configuration)

### Future Integrations

- **Job board direct integrations:** Improve ingestion quality after v1 validation *(post-v1)*
- **Calendar integrations:** Follow-up scheduling sync *(post-v1)*
- **OAuth providers (Google/LinkedIn):** Lower signup friction *(post-v1)*

---

## Contact Strategy

### Primary Contact Method

In-app support entry point with email-based response channel.

### Contact Channels

| Channel | Priority | Implementation |
|---------|----------|----------------|
| In-app support form | High | Form posts to backend endpoint; messages forwarded via SMTP/email workflow |
| Support email address | High | Published support inbox for direct contact |
| Knowledge FAQ page | Medium | Static help content in frontend |

### Future: AI Integration

No AI support assistant in v1. Reassess once support volume and issue taxonomy are known.

---

## UX Constraints

### Platform Limitations

- Browser-only experience in v1 (no native mobile apps)
- URL parsing is intentionally minimal in v1 to preserve delivery speed
- Email is the only notification channel in v1
- Auto-apply automation is out of scope for v1

### Performance Targets

| Metric | Target | Rationale |
|--------|--------|-----------|
| **Mobile First** | Responsive layouts for modern mobile browsers | Primary quick-update use case |
| **Page Load** | First meaningful load under 3s on typical 4G | Reduce friction for frequent status updates |
| **Offline Support** | Not supported in v1 | Defer complexity for faster launch |

---

## Multilingual Requirements

Single-language experience for v1 (English).

---

## SEO Requirements

### Technical SEO

- Index marketing/landing pages as available
- Exclude authenticated app routes from indexing
- Provide metadata defaults for public pages

### Structured Data

| Page Type | Schema Type | Key Properties |
|-----------|-------------|----------------|
| Marketing landing page | `WebSite` | name, description, url |

### Local SEO (if applicable)

Not a local business.

### Performance & Infrastructure

| Metric | Target |
|--------|--------|
| **Largest Contentful Paint (LCP)** | < 2.5 seconds |
| **First Input Delay (FID)** | < 100ms |
| **Cumulative Layout Shift (CLS)** | < 0.1 |
| **Page Load (4G)** | < 3 seconds |
| **Total Page Weight** | < 3MB |
| **Mobile-Friendly** | Yes |

### Security Headers

| Header | Purpose |
|--------|---------|
| **Strict-Transport-Security (HSTS)** | Force HTTPS |
| **Content-Security-Policy (CSP)** | Reduce XSS risk |
| **X-Content-Type-Options** | Prevent MIME sniffing |
| **X-Frame-Options** | Prevent clickjacking |
| **Referrer-Policy** | Control referrer leakage |

### SEO Plugin/Tools

No dedicated SEO plugin required for v1 app runtime.

---

## Authentication & Security Baseline

- **Auth model:** Email/password + JWT
- **Password policy:** Strong hash (e.g., BCrypt/Argon2), no plaintext storage
- **Session approach:** Short-lived access token, revocation strategy via backend controls
- **Auditability:** Record key account/security events and status transition changes

---

## Data Intake and Parsing

- **URL parsing strategy:** Minimal parse in v1 (title + source + URL)
- **Manual override:** User can edit parsed fields before save
- **Fallback:** Manual entry always available when parsing fails

---

## Notification Rules (v1)

- **Channel:** Email via SMTP
- **Triggers:**
  - Status change
  - Follow-up due
  - Stalled application (7 days without status change)

---

## Maintenance & Ownership

| Aspect | Owner | Notes |
|--------|-------|-------|
| **Content Updates** | JC | Product copy and help content managed directly |
| **Technical Maintenance** | JC | Solo ownership of stack, deployment, and fixes |
| **Dependency Updates** | JC | Regular patching cadence with security checks |

---

## Development Handoff Notes

### Environment Setup

- Docker Compose local/dev parity with VPS deployment
- Environment variables for DB connection, JWT secret, SMTP credentials
- Migration tooling executed on startup/deploy

### Deployment Process

- Build Flutter Web artifacts
- Build Spring Boot API image
- Deploy/update Docker Compose services on VPS
- Run migration checks and smoke tests

### Key Considerations

- Protect SMTP credentials and rotate secrets periodically
- Add retry/backoff for email delivery jobs
- Capture status history as append-only events for analytics integrity
- Keep API contracts stable for future native/mobile clients

---

## Next Steps

- [ ] **Trigger Mapping (TM)** — connect business goals to user motivation and decision friction
- [ ] **UX Scenarios (OS/SC)** — define flow-level interaction details and edge cases
- [ ] **Design Delivery (DD)** — prepare implementation-ready handoff package

---

_Generated for Web Design Studio planning flow_
