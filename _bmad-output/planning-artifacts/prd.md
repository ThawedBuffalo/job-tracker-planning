---
stepsCompleted:
  - step-01-init
  - step-02-discovery
  - step-02b-vision
  - step-02c-executive-summary
  - step-03-success
  - step-04-journeys
  - step-05-domain
  - step-06-innovation
  - step-07-project-type
  - step-08-scoping
  - step-09-functional
  - step-10-nonfunctional
  - step-11-polish
  - step-12-complete
inputDocuments:
  - design-artifacts/A-Product-Brief/project-brief.md
  - design-artifacts/A-Product-Brief/platform-requirements.md
  - design-artifacts/B-Trigger-Map/trigger-map.md
  - design-artifacts/B-Trigger-Map/feature-impact-analysis.md
  - design-artifacts/C-UX-Scenarios/00-ux-scenarios.md
  - design-artifacts/E-PRD/Design-Deliveries/DD-001-application-capture-and-follow-up.yaml
workflowType: prd
classification:
  projectType: web-app
  domain: productivity-tools
  complexity: medium
  projectContext: brownfield
documentCounts:
  briefCount: 2
  researchCount: 0
  brainstormingCount: 0
  projectDocsCount: 6
---

# Product Requirements Document — job-tracker

**Author:** JC
**Date:** 2026-03-27
**Version:** 1.0
**Status:** Draft — Pending Architect Review

---

## 1. Executive Summary

`job-tracker` is a focused, solo-engineer web application that replaces fragmented job-search tracking tools — spreadsheets, Notion docs, handwritten notes — with a single guided workflow for individual technical job seekers. It tracks every application from first identification through offer or rejection, prompts the user on what to do next, and sends email reminders when action is due or momentum has stalled.

**The core insight:** job seekers don't lose opportunities because they apply too few times — they lose them because tracking across tools breaks down, follow-ups slip, and there's no clear "what now?" signal after each stage. `job-tracker` solves this with a canonical 9-stage pipeline, required-field discipline, and proactive email notifications.

**Delivery target:** v1 in 1 month by a solo engineer using Flutter Web + Spring Boot + PostgreSQL on a single VPS.

**Primary success metric:** +25% applications per week vs. user's baseline within 6 months.

---

## 2. Product Vision

> A single, guided workspace where individual technical job seekers consistently move more opportunities forward each week — with clarity on every next action, no dropped follow-ups, and a complete history of every application.

### Positioning

| Dimension | Value |
|-----------|-------|
| **For** | Individual technical job seekers actively searching worldwide |
| **Who** | Manage multiple active applications across many sources |
| **The product is** | A centralized workflow and notification tool |
| **That** | Tracks every application stage and guides next steps |
| **Unlike** | Notion, Excel, handwritten notes |
| **Our advantage** | Purpose-built pipeline progression + proactive email notifications |

---

## 3. Success Criteria

### Primary Outcome (6 Months Post-Launch)

- Increase **applications submitted per week by 25%** relative to per-user baseline established at onboarding

### Supporting Metrics

| Metric | Direction | Notes |
|--------|-----------|-------|
| `identified → submitted` conversion rate | ↑ Increase | Measures pipeline momentum |
| `submitted → recruiter_screen` conversion rate | ↑ Increase | Measures follow-up quality |
| Median time: `identified → submitted` | ↓ Decrease | Measures entry friction |
| % applications with follow-up completed on time | ↑ Increase | Measures notification effectiveness |
| Daily active users (DAU) / Monthly active users (MAU) | ↑ Increase | Measures habit formation |

### Measurement Approach

- Capture baseline `applications/week` at onboarding (user self-reports or derived from first-week data)
- Track rolling 7-day and 30-day averages to smooth outliers
- Evaluate sustained uplift (4+ consecutive weeks above baseline) not one-week spikes

### MVP Validation Gate

The v1 is validated when a user who has actively used the system for 4 weeks shows measurable improvement in at least one pipeline metric vs. their stated baseline.

---

## 4. User Personas & Journeys

### Primary Persona: Priya — The Pipeline Optimizer

**Profile:** Senior software engineer, 6 YOE, actively interviewing. Runs 5–8 concurrent active applications. Uses desktop browser for focused evening job-search sessions.

**Core Journey:** Priya's Fast Job Entry
1. Opens bookmarked app URL → authenticates → lands on Dashboard
2. Selects "Add Application" → pastes job URL → app parses title + company + URL
3. Reviews auto-populated fields → fills required fields → submits
4. Confirmation displayed on Dashboard; next action suggested

**Driving Forces:**
- Want: Keep momentum with low-friction updates
- Fear: Losing track across tools

---

### Secondary Persona: Clara — The Career Changer

**Profile:** Experienced QA engineer pivoting to product management. Less familiar with the interview process for a new role type. Needs clear guidance at every stage.

**Core Journey:** Clara's Follow-Up Confidence Loop
1. Receives an email notification about a stalled application → clicks link → lands on Application Detail
2. Sees current status + guided "what's next" panel
3. Initiates status update → system suggests next action
4. Confirms action taken → status advances

**Driving Forces:**
- Hope: System provides clear next steps at every stage
- Fear: Missing a critical stage transition or follow-up window

---

### Secondary Persona: Ethan — The Efficient Explorer

**Profile:** Early-career dev applying at high volume (15+ applications/week). Speed is everything; won't tolerate friction.

**Core Journey:** Ethan's Required-Field Fast Entry
1. Pastes job URL → auto-populated fields displayed
2. System flags empty required fields before save
3. Ethan fills only required fields → saves
4. Application created at `identified` status; optional fields fillable later

**Driving Forces:**
- Hope: Enter data fast, don't miss required fields accidentally
- Fear: Incomplete records that cause problems later

---

### Tertiary Persona: Ravi — The Remote Reacher

**Profile:** DevOps engineer applying globally, surfing job boards on both desktop and mobile. Multiple browser tabs open simultaneously.

**Core Journey:** Ravi's Unified Global Entry
1. On mobile browser: copies job URL → opens `job-tracker` → pastes URL
2. URL parsed → fields reviewed → saves in under 60 seconds
3. Returns to job boards without losing his browsing flow

**Driving Forces:**
- Hope: Capture any job in under a minute from any browser/device
- Fear: App is clunky on mobile or makes him lose his tab flow

---

## 5. Domain Requirements

### Application Lifecycle Model

The canonical application status pipeline is the core domain model. All product decisions derive from it.

| # | Status | Meaning | Terminal? |
|---|--------|---------|-----------|
| 1 | `identified` | Found and logged; not yet reviewed | No |
| 2 | `reviewed` | JD read and assessed | No |
| 3 | `customized_resume` | Resume tailored for this role | No |
| 4 | `submitted` | Application formally submitted | No |
| 5 | `first_contact` | Recruiter or hiring manager has reached out | No |
| 6 | `recruiter_screen` | Recruiter screening call scheduled or completed | No |
| 7 | `tech_interview` | Technical interview stage | No |
| 8 | `offer` | Offer received | No |
| 9 | `closed` | Position filled or application withdrawn by user | **Yes** |
| — | `rejected` | Explicitly rejected by employer | **Yes** |

**Transition Rules:**
- Default direction: forward (sequential)
- Manual override allowed (corrections, out-of-order responses)
- Terminal statuses (`closed`, `rejected`) cannot be reversed without explicit user intent
- Every transition is recorded as an append-only audit event with timestamp

### Required Fields Per Application

The following fields are mandatory before an application can be saved:

| Field | Source | Notes |
|-------|--------|-------|
| `job_title` | URL parse or manual | Primary display label |
| `company_name` | URL parse or manual | Grouping and display |
| `job_url` | URL paste or manual | Source link |
| `status` | System default: `identified` | Initial value set on create |

Optional but encouraged fields (can be filled later):
- `notes` — free-text field
- `job_description` — pasted JD
- `salary_range` — text estimate
- `contact_name` / `contact_email` — recruiter or hiring manager
- `applied_date` — date submitted (defaults to `submitted` transition timestamp)
- `follow_up_date` — user-set follow-up reminder

### Follow-Up & Notification Rules

| Trigger | Channel | Timing |
|---------|---------|--------|
| Status change | Email | Immediately on transition |
| Follow-up date due | Email | On the date set by user |
| Stalled application | Email | 7 days with no status change (not in terminal status) |

---

## 6. Innovation Patterns

### Key Differentiators vs. Alternatives

| Pattern | Description | Impact |
|---------|-------------|--------|
| **Guided pipeline** | Every status has a defined "what's next" prompt | Reduces anxiety, increases stage progression |
| **URL intake** | Paste a job URL to pre-fill required fields | Eliminates the biggest manual friction point |
| **Required-field gate** | Cannot save without required fields; system flags missing fields inline | Eliminates incomplete records |
| **Proactive staleness detection** | 7-day stall alert prevents applications from falling off radar | Directly addresses the #1 reason for missed follow-ups |
| **Append-only audit trail** | Every status change timestamped and stored | Enables analytics, prevents data loss |

### What We're Deliberately NOT Doing (v1)

- No auto-apply automation
- No OAuth (Google/LinkedIn sign-in) — email/password only
- No native iOS/Android apps — responsive web only
- No job board direct integrations — URL paste is the intake method
- No AI features — guided workflow beats AI noise for v1
- No advanced analytics dashboard — pipeline visibility is sufficient for v1

---

## 7. Project Type Requirements

### Technology Stack

| Layer | Technology | Rationale |
|-------|------------|-----------|
| Frontend | Flutter Web | Shared skill set; responsive web + mobile web from one codebase |
| Backend API | Spring Boot (Java) | Mature auth + email + REST patterns; solo dev familiar |
| Database | PostgreSQL | Relational fits pipeline status model + audit history |
| Infrastructure | Docker Compose + Nginx + VPS | Simplest path to production for solo engineer |
| Authentication | Spring Security + JWT | Standard, well-documented, no external dependency |
| Email | JavaMailSender + SMTP | Provider-agnostic; swap SMTP without code change |
| URL Parsing | Jsoup | Lightweight HTML scraping; minimal in v1 |
| DB Migrations | Flyway | Auto-runs on deploy; append-only migration history |

### Architecture Pattern

- **Single VPS deployment** — Docker Compose manages all services
- **Monolith-first** — Single Spring Boot API in v1; no microservices
- **Database per app** — Single PostgreSQL instance, single schema
- **SMTP-agnostic notifications** — Configured via environment variable; swap provider without code change
- **JWT stateless auth** — Short-lived access tokens; no server-side session state
- **Append-only event log** — Status changes stored as events, not overwrites

---

## 8. MVP Scope

### In Scope — v1

| # | Feature Area | Description |
|---|-------------|-------------|
| 1 | User Authentication | Email/password sign-up, sign-in, JWT session, sign-out |
| 2 | Application Capture — Manual | Create application with required fields + optional fields |
| 3 | Application Capture — URL Intake | Paste URL → parse title + company + URL → pre-fill form |
| 4 | Pipeline Status Management | Advance/update application status through canonical 9-stage pipeline |
| 5 | Required Field Enforcement | Block save if required fields missing; highlight gaps inline |
| 6 | Dashboard | List all active applications with current status; quick-access actions |
| 7 | Application Detail View | Full application record + status history + notes + follow-up date |
| 8 | Next-Step Guidance | Context-sensitive prompt after each status update: "Next: do X" |
| 9 | Email Notifications | Stage-change email, follow-up due email, stall alert (7 days) |
| 10 | Status Audit History | View timestamped log of all status changes per application |
| 11 | Responsive Design | Full desktop-first layout; functional on mobile browsers |

### Out of Scope — Post-v1

| Feature | Reason Deferred |
|---------|----------------|
| OAuth (Google/LinkedIn) | Complexity vs. value ratio too high for v1 |
| Native iOS/Android apps | Responsive web covers mobile adequately for v1 |
| Job board direct integrations | URL paste is sufficient; integrations add API dependency risk |
| Auto-apply automation | Out of product scope |
| Advanced pipeline analytics | Not needed until user retention is established |
| AI suggestions / resume critique | Post-v1 enhancement after core value proven |
| Team/shared accounts | Solo job seeker is sole v1 persona |
| Calendar integrations | Post-v1; follow-up date field covers v1 need |

---

## 9. Functional Requirements

### FR Area 1 — User Account Management

| ID | Requirement |
|----|-------------|
| FR-001 | A visitor can register with email and password to create an account |
| FR-002 | A registered user can sign in with email and password |
| FR-003 | The system issues a short-lived JWT access token on successful authentication |
| FR-004 | An authenticated user can sign out, invalidating the active session |
| FR-005 | An authenticated user can update their account email or password |
| FR-006 | The system stores passwords using a strong hash (BCrypt/Argon2); no plaintext |

---

### FR Area 2 — Application Capture

| ID | Requirement |
|----|-------------|
| FR-010 | An authenticated user can create a new application record manually by entering required fields |
| FR-011 | An authenticated user can paste a job URL to initiate application capture |
| FR-012 | The system parses a pasted URL to extract and pre-populate: job title, company name, and source URL |
| FR-013 | The user can edit any auto-populated field before saving |
| FR-014 | The system blocks save and highlights missing fields if any required field is empty |
| FR-015 | A newly saved application defaults to `identified` status |
| FR-016 | An authenticated user can attach optional fields (notes, JD text, salary range, contact info, applied date, follow-up date) to any application |
| FR-017 | An authenticated user can edit an existing application record |
| FR-018 | An authenticated user can delete an application record |

---

### FR Area 3 — Pipeline Status Management

| ID | Requirement |
|----|-------------|
| FR-020 | An authenticated user can advance an application to the next stage in the canonical pipeline |
| FR-021 | An authenticated user can manually set an application to any non-terminal stage (override for corrections) |
| FR-022 | An authenticated user can mark an application as `rejected` |
| FR-023 | An authenticated user can mark an application as `closed` |
| FR-024 | Terminal status applications (`rejected`, `closed`) require explicit user intent to reverse |
| FR-025 | Every status change is recorded as an append-only audit event with timestamp and actor |
| FR-026 | The system displays a context-sensitive "next step" prompt after every status change |

---

### FR Area 4 — Dashboard & Visibility

| ID | Requirement |
|----|-------------|
| FR-030 | An authenticated user can view a dashboard listing all active applications with current status |
| FR-031 | The dashboard groups or sorts applications by status and recency |
| FR-032 | The dashboard visually distinguishes stalled applications (7+ days without change) |
| FR-033 | An authenticated user can view the full detail record of any application |
| FR-034 | An authenticated user can view the complete chronological status history of an application |
| FR-035 | An authenticated user can filter applications by status on the dashboard |
| FR-036 | An authenticated user can search applications by company name or job title |

---

### FR Area 5 — Email Notifications

| ID | Requirement |
|----|-------------|
| FR-040 | The system sends an email to the user when any application status changes |
| FR-041 | The system sends an email to the user when a user-set follow-up date is reached |
| FR-042 | The system sends an email to the user when an active application has had no status change for 7 days |
| FR-043 | Stall notifications are only sent for non-terminal applications |
| FR-044 | Email notifications include: application name, company, current status, and a direct link to the application detail |
| FR-045 | The user can opt out of individual notification types (stage-change, follow-up, stall) |
| FR-046 | The system retries failed email sends with exponential backoff |

---

### FR Area 6 — System & Data Integrity

| ID | Requirement |
|----|-------------|
| FR-050 | All authenticated API endpoints require a valid JWT; unauthenticated requests are rejected with 401 |
| FR-051 | Users can only access and modify their own application records |
| FR-052 | The system logs all security-relevant events (sign-in, sign-out, failed auth attempts) |
| FR-053 | The system captures the creation timestamp and last-modified timestamp for every application record |
| FR-054 | All status transition events are immutable once recorded |

---

## 10. Non-Functional Requirements

### Performance

| NFR | Target | Rationale |
|-----|--------|-----------|
| NFR-P-001 | Dashboard first meaningful load < 3s on 4G | Core daily-use screen; slow load kills habit |
| NFR-P-002 | API response time < 500ms (p95) under normal load | Solo user, low concurrency; highly achievable |
| NFR-P-003 | URL parse + form pre-fill < 2s | User is on the source page; delay feels broken |

### Reliability & Availability

| NFR | Target | Rationale |
|-----|--------|-----------|
| NFR-R-001 | 99.5% uptime (VPS single-node; some maintenance window allowed) | Solo engineer; no SLA to external stakeholders |
| NFR-R-002 | Email delivery: retry with backoff for up to 24 hours on failure | SMTP transient failures must not lose notifications |
| NFR-R-003 | Database backup: daily automated backup with 7-day retention | Data loss for job seeker is high impact |

### Security

| NFR | Target |
|-----|--------|
| NFR-S-001 | Password storage: BCrypt (cost ≥ 12) or Argon2 |
| NFR-S-002 | JWT: HS256 or RS256, expiry ≤ 15 minutes (access token) |
| NFR-S-003 | HTTPS enforced for all traffic via Nginx TLS termination |
| NFR-S-004 | Security headers: HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy |
| NFR-S-005 | SMTP credentials stored as environment variables, never in source |
| NFR-S-006 | All user data scoped by `user_id`; no cross-user data access permitted |

### Scalability

| NFR | Target | Rationale |
|-----|--------|-----------|
| NFR-SC-001 | Support single-user concurrent sessions without degradation | v1 is effectively single-user per account; shared infra |
| NFR-SC-002 | Database schema designed to support multi-user without redesign | Future-proofs toward multi-user without migration pain |

### Maintainability

| NFR | Target |
|-----|--------|
| NFR-M-001 | All DB schema changes managed through Flyway migrations |
| NFR-M-002 | Docker Compose environment parity between local dev and VPS |
| NFR-M-003 | All environment-specific values (DB, SMTP, JWT secret) configurable via environment variables |
| NFR-M-004 | API errors return structured JSON with error code and message |

### Accessibility & UX

| NFR | Target |
|-----|--------|
| NFR-A-001 | Responsive layout functional on desktop and modern mobile browsers |
| NFR-A-002 | Page load under 3MB total weight |
| NFR-A-003 | LCP < 2.5s, CLS < 0.1, FID < 100ms |
| NFR-A-004 | English only (v1) |
| NFR-A-005 | WCAG 2.1 AA compliance for core application flows |

---

## 11. Epics Overview

The following epics organize all functional requirements into deliverable units. Each epic maps directly to an implementation sprint or phase.

| Epic | Name | FRs Covered | Priority |
|------|------|-------------|----------|
| E-001 | Auth & Account | FR-001–006, FR-050–052 | P0 — Must ship |
| E-002 | Application Capture | FR-010–018 | P0 — Must ship |
| E-003 | URL Intake & Parsing | FR-011–013 | P0 — Must ship |
| E-004 | Pipeline Management | FR-020–026 | P0 — Must ship |
| E-005 | Dashboard & Visibility | FR-030–036 | P0 — Must ship |
| E-006 | Email Notifications | FR-040–046 | P0 — Must ship |
| E-007 | Data Integrity & Audit | FR-053–054, NFR-M-001 | P0 — Must ship |
| E-008 | Infrastructure & DevOps | NFR-M-002, NFR-M-003, NFR-S-003 | P0 — Must ship (enable delivery) |

### Epic E-001: Auth & Account

**Goal:** Allow users to register, sign in, and securely access their data.

**Stories (draft headings — SM agent elaborates):**
- STORY-001: User registration (email + password)
- STORY-002: User sign-in with JWT issuance
- STORY-003: User sign-out / token invalidation
- STORY-004: Password change
- STORY-005: Security headers + HTTPS enforcement

---

### Epic E-002: Application Capture

**Goal:** Allow users to create and manage application records manually with required-field enforcement.

**Stories:**
- STORY-010: Create application (manual entry, required fields)
- STORY-011: Edit existing application record
- STORY-012: Delete application
- STORY-013: Required-field validation (block save + inline error)
- STORY-014: Add/edit optional fields (notes, JD, salary, contact, dates)

---

### Epic E-003: URL Intake & Parsing

**Goal:** Reduce data-entry friction by parsing a job URL into pre-populated form fields.

**Stories:**
- STORY-020: URL paste intake flow (UI entry point)
- STORY-021: Backend URL parse endpoint (Jsoup, returns title + company + url)
- STORY-022: Map parsed fields to application form
- STORY-023: Manual override of parsed fields before save
- STORY-024: Graceful fallback when parsing fails (manual entry mode)

---

### Epic E-004: Pipeline Management

**Goal:** Enable users to advance applications through the canonical 9-stage pipeline with guided next-step prompts.

**Stories:**
- STORY-030: Status advance (next stage in canonical order)
- STORY-031: Manual status override (any non-terminal stage)
- STORY-032: Mark as `rejected`
- STORY-033: Mark as `closed`
- STORY-034: Append-only status event recording
- STORY-035: Context-sensitive "next step" prompt after status change

---

### Epic E-005: Dashboard & Visibility

**Goal:** Give users a clear overview of their entire pipeline with fast access to any application.

**Stories:**
- STORY-040: Dashboard — active application list with status badges
- STORY-041: Dashboard — stalled application visual indicator (7+ days)
- STORY-042: Dashboard — sort by status / recency
- STORY-043: Application detail view (all fields + status history)
- STORY-044: Dashboard — filter by status
- STORY-045: Dashboard — search by company/title

---

### Epic E-006: Email Notifications

**Goal:** Keep users informed and prompt action via email at key pipeline moments.

**Stories:**
- STORY-050: Email on status change
- STORY-051: Email on follow-up date reached
- STORY-052: Email on stalled application (7-day check)
- STORY-053: Notification content (application name, company, status, direct link)
- STORY-054: Notification opt-out preferences
- STORY-055: Email retry with backoff

---

### Epic E-007: Data Integrity & Audit

**Goal:** Ensure data quality and traceability across all application records.

**Stories:**
- STORY-060: Status event immutability (append-only events table)
- STORY-061: Record created_at / updated_at timestamps
- STORY-062: User-scoped data access (no cross-user reads/writes)

---

### Epic E-008: Infrastructure & DevOps

**Goal:** Enable fast, repeatable deployment on a single VPS with environment parity.

**Stories:**
- STORY-070: Docker Compose local dev setup (Flutter dev server, Spring Boot, PostgreSQL)
- STORY-071: Docker Compose VPS production stack (Nginx TLS, Spring Boot, PostgreSQL)
- STORY-072: Flyway migration baseline + first schema migration
- STORY-073: Environment variable configuration for all secrets
- STORY-074: Automated daily PostgreSQL backup

---

## 12. Open Questions & Decisions

| # | Question | Owner | Status |
|---|----------|-------|--------|
| OQ-001 | JWT refresh token strategy — silent refresh vs. re-login on expiry? | JC + Architect | Open |
| OQ-002 | SMTP provider selection — SendGrid, Postmark, or self-hosted? | JC | Open |
| OQ-003 | URL parsing fallback UX — modal with manual fields, or inline form switch? | JC + UX (Freya) | Open |
| OQ-004 | Stale notification frequency — once per 7 days, or repeat every 7 days until resolved? | JC | Open |
| OQ-005 | Dashboard default sort — most recent status change, or alphabetical? | JC | Open |
| OQ-006 | Mobile-first vs desktop-first Flutter layout strategy? | JC + Architect | Decided: Desktop-first (per Platform Requirements) |

---

## 13. Reference Documents

| Document | Location | Role in PRD |
|----------|----------|-------------|
| Product Brief | `design-artifacts/A-Product-Brief/project-brief.md` | Strategic foundation, MVP scope, personas |
| Platform Requirements | `design-artifacts/A-Product-Brief/platform-requirements.md` | Stack decisions, NFRs, deployment |
| Trigger Map | `design-artifacts/B-Trigger-Map/trigger-map.md` | User psychology, feature prioritization |
| Feature Impact Analysis | `design-artifacts/B-Trigger-Map/feature-impact-analysis.md` | Epic prioritization rationale |
| UX Scenarios | `design-artifacts/C-UX-Scenarios/00-ux-scenarios.md` | Journey source for FR derivation |
| Design Deliveries | `design-artifacts/E-PRD/Design-Deliveries/DD-001-application-capture-and-follow-up.yaml` | UX handoff specs |

---

## 14. Next Steps

- [ ] **Architect Review** — Validate stack decisions, API contract, data model against FRs
- [ ] **Architecture Doc** — Activate `*architect`; produce system architecture, OpenAPI spec, PostgreSQL schema
- [ ] **Epic & Story Elaboration** — Activate `*sm`; expand epic headings into full story files with acceptance criteria
- [ ] **Implementation** — Per-repo story-driven development (Flutter, Spring Boot)
- [ ] **QA** — Per-story acceptance criteria validation

---

_Generated by John (BMad PM Agent) · job-tracker-planning · 2026-03-27_
_Input artifacts: Product Brief, Platform Requirements, Trigger Map, Feature Impact Analysis, UX Scenarios (4 × 4 pages), Design Deliveries_

