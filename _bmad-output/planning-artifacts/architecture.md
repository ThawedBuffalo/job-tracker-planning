---
stepsCompleted:
  - step-01-init
  - step-02-context
  - step-03-starter
  - step-04-decisions
  - step-05-patterns
  - step-06-structure
  - step-07-validation
  - step-08-complete
inputDocuments:
  - _bmad-output/planning-artifacts/prd.md
  - design-artifacts/A-Product-Brief/project-brief.md
  - design-artifacts/A-Product-Brief/platform-requirements.md
  - design-artifacts/B-Trigger-Map/trigger-map.md
  - design-artifacts/E-PRD/Design-Deliveries/DD-001-application-capture-and-follow-up.yaml
workflowType: architecture
project_name: job-tracker
user_name: JC
date: 2026-03-27
status: Superseded
supersededBy: _bmad-output/planning-artifacts/architecture-polyrepo.md
---

# Architecture Document — job-tracker (Superseded)

> This monolith baseline is retained for historical reference only.
> Active architecture source of truth: `_bmad-output/planning-artifacts/architecture-polyrepo.md`.

**Author:** JC
**Architect Agent:** Winston
**Date:** 2026-03-27
**Version:** 1.0
**Status:** Draft — Ready for Implementation

---

## 1. Project Context

### Summary

`job-tracker` is a **medium-complexity**, **single-VPS full-stack web application** for individual technical job seekers. It is a **greenfield** project with a fixed 1-month delivery constraint for a **solo engineer**.

The dominant architectural concern is **delivery velocity**: every design choice must favor simplicity, well-trodden patterns, and low operational complexity over theoretical scalability.

### Scale Indicators

| Dimension | Assessment |
|-----------|------------|
| Concurrency | Single active user per account; effectively single-user per session |
| Data volume | Low — hundreds of application records per user |
| Real-time requirements | None — email notifications cover all async needs |
| External integrations | SMTP only (v1) |
| Regulatory compliance | None |
| Team size | Solo engineer |

### Functional Requirement Summary (from PRD)

| Area | FR Count | Key Complexity |
|------|----------|---------------|
| Auth & Account | 6 | JWT with refresh, BCrypt hashing |
| Application Capture | 9 | URL parsing, required-field gate |
| Pipeline Management | 7 | Append-only status events, next-step guidance |
| Dashboard & Visibility | 7 | Filtering, search, stale detection |
| Email Notifications | 7 | SMTP, retry/backoff, stale job (async) |
| Data Integrity | 5 | Event immutability, user-scoped access |

---

## 2. Architecture Decisions

### ADR-001: Overall Architecture Style

**Decision:** Single-process monolith

**Options considered:**
- Monolith (chosen)
- Modular monolith
- Microservices

**Rationale:** Solo engineer + 1-month timeline + single active user = zero justification for service decomposition. A monolith ships faster, is easier to debug, and requires no service mesh, network coordination, or distributed tracing. The code will be **modular by package** (not by process), so extraction is possible post-v1 if needed.

**Consequences:** Simple deployment, fast iteration, low operational cost. Accept: if user base grows, a future split of notification processing into a separate worker is the natural first decomposition point.

---

### ADR-002: API Style

**Decision:** REST / JSON over HTTP

**Options considered:**
- REST JSON (chosen)
- GraphQL
- gRPC

**Rationale:** REST is well-supported by Spring Boot, easy to test manually with curl/Postman, and Flutter's `http` package handles it natively. GraphQL adds schema complexity with no query-flexibility benefit for this use case. gRPC is not supported in Flutter Web.

**API base path:** `/api/v1/`

---

### ADR-003: Authentication Strategy

**Decision:** Email/password → JWT access token (15 min) + refresh token (7 days) stored in HttpOnly cookie

**Options considered:**
- JWT access token only, no refresh (chosen baseline, rejected)
- Access + refresh tokens (chosen)
- Session cookies (rejected — stateful)
- OAuth only (rejected — post-v1)

**Rationale:**
- 15-minute access tokens limit blast radius of token theft
- Refresh token in HttpOnly cookie is not accessible to JavaScript (XSS mitigation)
- Silent refresh: Flutter Web calls `POST /api/v1/auth/refresh` when access token is near expiry; user never notices
- `POST /api/v1/auth/logout` clears the refresh cookie and invalidates server-side (store invalidated JTI in a `revoked_tokens` table or use Redis in future; for v1, clearing cookie is sufficient — accept the 15-min window risk)

**Resolves OQ-001** (JWT refresh token strategy)

---

### ADR-004: Password Security

**Decision:** BCrypt with cost factor 12

**Rationale:** BCrypt at cost 12 is slow enough to defeat brute-force attacks at typical v1 load levels, well-supported by Spring Security, and widely understood. Argon2 is theoretically superior but requires additional library configuration; defer to post-v1 if security audit recommends upgrade.

---

### ADR-005: Database

**Decision:** PostgreSQL 16, managed via Flyway migrations, single schema `job_tracker`

**Key design principles:**
- All FKs with `ON DELETE CASCADE` where child records have no meaning without parent
- `status` column uses a PostgreSQL `ENUM` type for pipeline statuses (validated at DB layer)
- `application_events` is append-only — no UPDATE or DELETE ever issued by application code
- All timestamps in UTC; application layer converts to user locale display only
- `JSONB` used for notification preferences (flexible schema, no migration needed for new notification types)

**Resolves:** NFR-M-001 (Flyway), FR-054 (immutable events)

---

### ADR-006: Email Notifications

**Decision:** JavaMailSender via SMTP; provider = Postmark (free tier: 100 emails/month)

**Rationale:**
- Postmark has excellent deliverability, a simple SMTP interface, and a generous free tier sufficient for v1 single-user load
- Configuration is provider-agnostic — swap to SendGrid, SES, or self-hosted with only env var changes
- Scheduled stale-check job runs via Spring `@Scheduled` every 24 hours at 08:00 UTC
- Failed sends are retried up to 3 times with 5-minute exponential backoff using Spring Retry

**Resolves OQ-002** (SMTP provider selection)

---

### ADR-007: URL Parsing

**Decision:** Jsoup fetches the job URL server-side; extracts `<title>` + OpenGraph `og:site_name` / `og:title`; returns partial result on failure

**Fallback UX (resolves OQ-003):**
- Parsed fields pre-fill the "Add Application" form
- If parsing partially fails (e.g., title found but company not), available fields are pre-filled; empty fields are left blank with placeholder text "Could not detect — enter manually"
- If the URL is unreachable or Jsoup returns nothing, the form opens in full manual mode with an inline toast: "Couldn't read that URL. Enter details manually."
- User can always edit all fields regardless of parse result

**Server-side parsing rationale:** Avoids CORS issues that would block a client-side fetch of arbitrary job board URLs.

---

### ADR-008: Stale Notification Strategy

**Decision:** Repeat every 7 days until application reaches a terminal status or user updates the record

**Rationale:** A single notification is easy to miss or forget. Repeating every 7 days creates gentle, persistent accountability without being spammy. The user silences it by taking action (updating status) or by opting out in notification preferences.

**Resolves OQ-004**

---

### ADR-009: Dashboard Default Sort

**Decision:** Sort by `last_status_changed_at` descending (most recently active at top)

**Rationale:** The most recently active application is the most likely to need attention. Alphabetical sort has no workflow value. Status-group sort is useful but adds complexity; defer to a later iteration where user can choose.

**Resolves OQ-005**

---

### ADR-010: Flutter Layout Strategy

**Decision:** Desktop-first with responsive breakpoints for mobile

**Breakpoints:**
- Desktop: ≥ 960px (full sidebar + main content two-column)
- Tablet: 600px–959px (collapsible nav + single column)
- Mobile: < 600px (bottom navigation + single column)

**Rationale:** Confirmed in Platform Requirements. Primary use case is desktop evening sessions; mobile is a secondary quick-update use case.

**Resolves OQ-006**

---

## 3. Technology Stack (Pinned Versions)

| Layer | Technology | Version | Notes |
|-------|------------|---------|-------|
| Frontend | Flutter (Web) | 3.29.x | Stable channel |
| Frontend state | flutter_bloc | 9.x | BLoC pattern for predictable state |
| Frontend HTTP | dio | 5.x | Interceptors for JWT auto-refresh |
| Frontend routing | go_router | 14.x | Declarative routing |
| Frontend UI | Material 3 | via Flutter SDK | Custom theme tokens |
| Backend | Java | 21 LTS | Virtual threads (Project Loom) |
| Backend framework | Spring Boot | 3.4.x | |
| Spring Security | spring-security | 6.4.x | JWT filter chain |
| JWT | jjwt | 0.12.x | `io.jsonwebtoken` |
| Email | spring-boot-starter-mail | 3.4.x | JavaMailSender |
| Spring Retry | spring-retry | 2.x | Email backoff |
| URL parsing | jsoup | 1.18.x | |
| DB migration | Flyway | 10.x | Auto-runs on startup |
| Database | PostgreSQL | 16.x | |
| JDBC | spring-boot-starter-data-jpa | 3.4.x | Hibernate ORM |
| Reverse proxy | Nginx | 1.27.x | TLS termination, static Flutter assets |
| Container | Docker Compose | 2.x | |
| OS | Ubuntu 24.04 LTS | | VPS target |

---

## 4. Database Schema

### Schema: `job_tracker`

---

#### Table: `users`

```sql
CREATE TABLE users (
    id                        UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email                     VARCHAR(255) NOT NULL UNIQUE,
    password_hash             VARCHAR(255) NOT NULL,
    notification_preferences  JSONB NOT NULL DEFAULT '{"stage_change": true, "follow_up_due": true, "stale_application": true}',
    created_at                TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at                TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
```

---

#### Enum: `application_status`

```sql
CREATE TYPE application_status AS ENUM (
    'identified',
    'reviewed',
    'customized_resume',
    'submitted',
    'first_contact',
    'recruiter_screen',
    'tech_interview',
    'offer',
    'closed',
    'rejected'
);
```

---

#### Table: `applications`

```sql
CREATE TABLE applications (
    id                    UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id               UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    job_title             VARCHAR(255) NOT NULL,
    company_name          VARCHAR(255) NOT NULL,
    job_url               TEXT NOT NULL,
    status                application_status NOT NULL DEFAULT 'identified',
    notes                 TEXT,
    job_description       TEXT,
    salary_range          VARCHAR(100),
    contact_name          VARCHAR(255),
    contact_email         VARCHAR(255),
    applied_date          DATE,
    follow_up_date        DATE,
    last_status_changed_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    created_at            TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at            TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_applications_user_id ON applications(user_id);
CREATE INDEX idx_applications_user_status ON applications(user_id, status);
CREATE INDEX idx_applications_last_status_changed ON applications(user_id, last_status_changed_at DESC);
CREATE INDEX idx_applications_follow_up_date ON applications(follow_up_date) WHERE follow_up_date IS NOT NULL;
```

---

#### Table: `application_events` (append-only)

```sql
CREATE TABLE application_events (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    application_id  UUID NOT NULL REFERENCES applications(id) ON DELETE CASCADE,
    user_id         UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    from_status     application_status,          -- NULL for initial 'identified' event
    to_status       application_status NOT NULL,
    note            TEXT,
    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- No UPDATE or DELETE allowed on this table (enforced via application code + DB rules)
CREATE INDEX idx_events_application_id ON application_events(application_id, created_at DESC);
```

---

#### Table: `notification_log`

```sql
CREATE TABLE notification_log (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id           UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    application_id    UUID REFERENCES applications(id) ON DELETE SET NULL,
    notification_type VARCHAR(50) NOT NULL,   -- 'stage_change' | 'follow_up_due' | 'stale_application'
    status            VARCHAR(20) NOT NULL DEFAULT 'pending',  -- 'pending' | 'sent' | 'failed'
    attempt_count     INTEGER NOT NULL DEFAULT 0,
    last_attempted_at TIMESTAMPTZ,
    sent_at           TIMESTAMPTZ,
    created_at        TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_notification_log_pending ON notification_log(status, created_at) WHERE status IN ('pending', 'failed');
```

---

### Flyway Migration Order

| Version | Description |
|---------|-------------|
| V1 | Create `users`, `application_status` enum |
| V2 | Create `applications` with all columns and indexes |
| V3 | Create `application_events` |
| V4 | Create `notification_log` |
| V5 | Add `last_status_changed_at` trigger function (auto-updates on status change) |

---

## 5. API Contract

### Base URL: `/api/v1`

All authenticated endpoints require: `Authorization: Bearer <access_token>`

---

### Auth

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| `POST` | `/auth/register` | Public | Register with email + password |
| `POST` | `/auth/login` | Public | Returns access token; sets refresh cookie |
| `POST` | `/auth/logout` | Required | Clears refresh cookie |
| `POST` | `/auth/refresh` | Cookie | Issues new access token via refresh cookie |

**POST /auth/register**
```json
// Request
{ "email": "user@example.com", "password": "s3cureP@ss" }

// Response 201
{ "id": "uuid", "email": "user@example.com", "created_at": "2026-03-27T..." }

// Error 400 (validation)
{ "error": "VALIDATION_ERROR", "message": "Password must be at least 8 characters", "field": "password" }

// Error 409 (duplicate)
{ "error": "EMAIL_ALREADY_REGISTERED", "message": "An account with this email already exists" }
```

**POST /auth/login**
```json
// Request
{ "email": "user@example.com", "password": "s3cureP@ss" }

// Response 200
{ "access_token": "eyJ...", "expires_in": 900 }
// + Set-Cookie: refresh_token=...; HttpOnly; Secure; SameSite=Strict; Path=/api/v1/auth/refresh; Max-Age=604800

// Error 401
{ "error": "INVALID_CREDENTIALS", "message": "Email or password is incorrect" }
```

---

### User Account

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| `GET` | `/users/me` | Required | Get current user profile |
| `PUT` | `/users/me` | Required | Update email or password |
| `GET` | `/users/me/notification-preferences` | Required | Get notification settings |
| `PUT` | `/users/me/notification-preferences` | Required | Update notification settings |

---

### Applications

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| `GET` | `/applications` | Required | List all applications |
| `POST` | `/applications` | Required | Create application |
| `GET` | `/applications/{id}` | Required | Get application detail |
| `PUT` | `/applications/{id}` | Required | Update application fields |
| `DELETE` | `/applications/{id}` | Required | Delete application |
| `POST` | `/applications/{id}/status` | Required | Advance or change status |
| `GET` | `/applications/{id}/events` | Required | Get status history |

**GET /applications — Query Parameters**

| Param | Type | Description |
|-------|------|-------------|
| `status` | string | Filter by status (e.g. `submitted`) |
| `q` | string | Search by job_title or company_name |
| `sort` | string | `last_status_changed_at` (default) \| `created_at` \| `company_name` |
| `order` | string | `desc` (default) \| `asc` |
| `page` | int | Page number (0-indexed) |
| `size` | int | Page size (default 20, max 100) |

**GET /applications Response**
```json
{
  "content": [
    {
      "id": "uuid",
      "job_title": "Senior Flutter Engineer",
      "company_name": "Acme Corp",
      "job_url": "https://...",
      "status": "submitted",
      "last_status_changed_at": "2026-03-25T10:00:00Z",
      "follow_up_date": "2026-04-01",
      "is_stale": false,
      "created_at": "2026-03-24T..."
    }
  ],
  "page": 0,
  "size": 20,
  "total_elements": 45,
  "total_pages": 3
}
```

**POST /applications Request**
```json
{
  "job_title": "Senior Flutter Engineer",      // required
  "company_name": "Acme Corp",                 // required
  "job_url": "https://jobs.acme.com/123",      // required
  "notes": "Referral from Alice",              // optional
  "job_description": "...",                    // optional
  "salary_range": "$130k–$160k",              // optional
  "contact_name": "Bob Recruiter",             // optional
  "contact_email": "bob@acme.com",             // optional
  "applied_date": "2026-03-27",               // optional
  "follow_up_date": "2026-04-03"              // optional
}
```

**POST /applications/{id}/status Request**
```json
{ "status": "recruiter_screen", "note": "Phone screen scheduled for April 2" }

// Response 200
{
  "id": "uuid",
  "status": "recruiter_screen",
  "last_status_changed_at": "2026-03-27T...",
  "next_step_prompt": "Prepare role-specific talking points. Research the company's engineering blog."
}
```

---

### URL Parsing

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| `POST` | `/parse-url` | Required | Parse a job URL into application fields |

**POST /parse-url**
```json
// Request
{ "url": "https://jobs.acme.com/posting/12345" }

// Response 200 — full parse
{ "job_title": "Senior Flutter Engineer", "company_name": "Acme Corp", "job_url": "https://..." }

// Response 200 — partial parse
{ "job_title": "Senior Flutter Engineer", "company_name": null, "job_url": "https://...", "parse_warnings": ["company_name could not be detected"] }

// Response 200 — parse failure
{ "job_title": null, "company_name": null, "job_url": "https://...", "parse_warnings": ["Could not read the URL. Enter details manually."] }
```

---

### Standard Error Response

All error responses follow this envelope:

```json
{
  "error": "ERROR_CODE",
  "message": "Human-readable description for display",
  "field": "field_name_if_validation_error",   // optional
  "request_id": "uuid"
}
```

| HTTP Status | When Used |
|-------------|-----------|
| 200 | Success |
| 201 | Resource created |
| 400 | Validation failure |
| 401 | Not authenticated |
| 403 | Authenticated but not authorized for this resource |
| 404 | Resource not found |
| 409 | Conflict (duplicate email, invalid state transition) |
| 422 | Unprocessable entity (e.g. invalid status transition direction) |
| 500 | Unexpected server error |

---

## 6. Implementation Patterns & Consistency Rules

### Naming Conventions

| Layer | Convention | Example |
|-------|------------|---------|
| DB tables | `snake_case`, plural | `applications`, `application_events` |
| DB columns | `snake_case` | `job_title`, `created_at` |
| DB indexes | `idx_{table}_{columns}` | `idx_applications_user_id` |
| Java packages | `com.jobtracker.{module}` | `com.jobtracker.applications` |
| Java classes | `PascalCase` | `ApplicationService`, `StatusChangeEvent` |
| Java methods | `camelCase` | `findByUserId`, `advanceStatus` |
| REST endpoints | `kebab-case`, plural nouns | `/applications`, `/parse-url` |
| REST JSON fields | `snake_case` | `job_title`, `created_at` |
| Flutter files | `snake_case.dart` | `application_card.dart` |
| Flutter classes | `PascalCase` | `ApplicationCard`, `DashboardBloc` |
| Flutter routes | `kebab-case` | `/dashboard`, `/applications/:id` |
| Env variables | `UPPER_SNAKE_CASE` | `DB_PASSWORD`, `SMTP_HOST` |

---

### Spring Boot Package Structure

```
com.jobtracker
├── auth/
│   ├── AuthController.java
│   ├── AuthService.java
│   ├── JwtService.java
│   ├── JwtAuthFilter.java
│   └── dto/
│       ├── RegisterRequest.java
│       ├── LoginRequest.java
│       └── AuthResponse.java
├── applications/
│   ├── ApplicationController.java
│   ├── ApplicationService.java
│   ├── ApplicationRepository.java
│   ├── ApplicationEventRepository.java
│   ├── model/
│   │   ├── Application.java
│   │   ├── ApplicationEvent.java
│   │   └── ApplicationStatus.java   (enum)
│   └── dto/
│       ├── CreateApplicationRequest.java
│       ├── UpdateApplicationRequest.java
│       ├── StatusChangeRequest.java
│       ├── ApplicationResponse.java
│       └── ApplicationListResponse.java
├── users/
│   ├── UserController.java
│   ├── UserService.java
│   ├── UserRepository.java
│   ├── model/
│   │   └── User.java
│   └── dto/
│       ├── UpdateUserRequest.java
│       ├── UserResponse.java
│       └── NotificationPreferences.java
├── notifications/
│   ├── NotificationScheduler.java       (@Scheduled stale check)
│   ├── NotificationService.java
│   ├── EmailSender.java
│   ├── NotificationLogRepository.java
│   └── model/
│       └── NotificationLog.java
├── parsing/
│   ├── UrlParseController.java
│   ├── UrlParseService.java
│   └── dto/
│       └── ParseUrlResponse.java
└── common/
    ├── exception/
    │   ├── GlobalExceptionHandler.java  (@ControllerAdvice)
    │   ├── ResourceNotFoundException.java
    │   ├── UnauthorizedException.java
    │   └── ValidationException.java
    ├── dto/
    │   └── ErrorResponse.java
    └── security/
        └── SecurityConfig.java
```

---

### Flutter Project Structure

```
lib/
├── main.dart
├── app.dart                        (MaterialApp + router setup)
├── core/
│   ├── api/
│   │   ├── api_client.dart         (Dio instance + interceptors)
│   │   ├── auth_interceptor.dart   (JWT attach + silent refresh)
│   │   └── api_exception.dart
│   ├── models/
│   │   ├── application.dart
│   │   ├── application_event.dart
│   │   ├── application_status.dart (enum)
│   │   └── user.dart
│   ├── router/
│   │   └── app_router.dart         (go_router config)
│   ├── theme/
│   │   ├── app_theme.dart
│   │   └── app_colors.dart
│   └── storage/
│       └── secure_storage.dart     (access token storage)
├── features/
│   ├── auth/
│   │   ├── bloc/
│   │   │   ├── auth_bloc.dart
│   │   │   ├── auth_event.dart
│   │   │   └── auth_state.dart
│   │   ├── repository/
│   │   │   └── auth_repository.dart
│   │   └── view/
│   │       ├── login_page.dart
│   │       └── register_page.dart
│   ├── dashboard/
│   │   ├── bloc/
│   │   ├── repository/
│   │   └── view/
│   │       ├── dashboard_page.dart
│   │       └── widgets/
│   │           ├── application_card.dart
│   │           └── stale_badge.dart
│   ├── application_form/
│   │   ├── bloc/
│   │   ├── repository/
│   │   └── view/
│   │       ├── add_application_page.dart
│   │       └── widgets/
│   │           ├── url_paste_field.dart
│   │           └── required_field_indicator.dart
│   ├── application_detail/
│   │   ├── bloc/
│   │   ├── repository/
│   │   └── view/
│   │       ├── application_detail_page.dart
│   │       └── widgets/
│   │           ├── status_timeline.dart
│   │           └── next_step_banner.dart
│   └── settings/
│       └── view/
│           └── notification_preferences_page.dart
└── shared/
    └── widgets/
        ├── primary_button.dart
        ├── error_banner.dart
        ├── loading_overlay.dart
        └── status_chip.dart
```

---

### Docker Compose & Infrastructure

```yaml
# docker-compose.yml (VPS production)
services:
  nginx:
    image: nginx:1.27-alpine
    ports: ["80:80", "443:443"]
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/certs:/etc/nginx/certs
      - flutter_build:/usr/share/nginx/html   # Flutter web build artifacts
    depends_on: [api]

  api:
    build: ./backend
    environment:
      DB_HOST: db
      DB_PORT: 5432
      DB_NAME: job_tracker
      DB_USER: ${DB_USER}
      DB_PASSWORD: ${DB_PASSWORD}
      JWT_SECRET: ${JWT_SECRET}
      JWT_ACCESS_EXPIRY_SECONDS: 900
      JWT_REFRESH_EXPIRY_SECONDS: 604800
      SMTP_HOST: smtp.postmarkapp.com
      SMTP_PORT: 587
      SMTP_USER: ${SMTP_USER}
      SMTP_PASSWORD: ${SMTP_PASSWORD}
      MAIL_FROM: noreply@yourdomain.com
      APP_BASE_URL: https://yourdomain.com
    depends_on: [db]
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: job_tracker
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

volumes:
  postgres_data:
  flutter_build:
```

**Nginx Config (key directives):**
```nginx
server {
    listen 443 ssl http2;
    ssl_certificate     /etc/nginx/certs/fullchain.pem;
    ssl_certificate_key /etc/nginx/certs/privkey.pem;

    # Serve Flutter Web SPA
    location / {
        root /usr/share/nginx/html;
        try_files $uri $uri/ /index.html;   # SPA fallback
    }

    # Proxy API
    location /api/ {
        proxy_pass http://api:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

# HTTP → HTTPS redirect
server { listen 80; return 301 https://$host$request_uri; }
```

---

### Security Headers (Nginx)

```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'" always;
add_header X-Frame-Options DENY always;
add_header X-Content-Type-Options nosniff always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
```

---

## 7. Implementation Patterns

### Spring Boot: Standard Service Pattern

Every service method that modifies state follows this pattern:

```java
@Transactional
public ApplicationResponse advanceStatus(UUID userId, UUID applicationId, StatusChangeRequest req) {
    // 1. Load and verify ownership
    Application app = applicationRepository.findByIdAndUserId(applicationId, userId)
        .orElseThrow(() -> new ResourceNotFoundException("Application not found"));

    // 2. Validate state transition
    validateTransition(app.getStatus(), req.getStatus());

    // 3. Record event (append-only — never update/delete)
    applicationEventRepository.save(new ApplicationEvent(
        applicationId, userId, app.getStatus(), req.getStatus(), req.getNote()
    ));

    // 4. Update entity
    app.setStatus(req.getStatus());
    app.setLastStatusChangedAt(Instant.now());
    applicationRepository.save(app);

    // 5. Trigger async notification
    notificationService.queueStageChangeNotification(userId, applicationId, req.getStatus());

    // 6. Return response with next-step prompt
    return ApplicationResponse.from(app, nextStepService.getPrompt(req.getStatus()));
}
```

---

### Flutter: BLoC Event→State Flow

All stateful features use `flutter_bloc`. Pattern:

```
UserAction → BlocEvent → BLoC processes → BlocState → Widget rebuilds
```

Example — status advance:
```
StatusAdvanceButtonTapped(status) →
  ApplicationBloc.onStatusAdvance →
    await repository.advanceStatus(id, status) →
      ApplicationUpdated(updatedApplication) |
      ApplicationError(message)
```

**No raw `setState()` in feature widgets.** All state changes flow through BLoC.

---

### JWT Silent Refresh (Dio Interceptor)

```dart
// auth_interceptor.dart
class AuthInterceptor extends Interceptor {
  @override
  void onRequest(RequestOptions options, RequestInterceptorHandler handler) async {
    final token = await secureStorage.readAccessToken();
    if (token != null) {
      options.headers['Authorization'] = 'Bearer $token';
    }
    handler.next(options);
  }

  @override
  void onError(DioException err, ErrorInterceptorHandler handler) async {
    if (err.response?.statusCode == 401) {
      // Attempt silent refresh
      try {
        final newToken = await authRepository.refresh();
        await secureStorage.writeAccessToken(newToken);
        // Retry original request
        handler.resolve(await _retry(err.requestOptions));
      } catch (_) {
        // Refresh failed → redirect to login
        authBloc.add(SessionExpired());
        handler.next(err);
      }
    } else {
      handler.next(err);
    }
  }
}
```

---

### Next-Step Prompts

The `next_step_prompt` in status change responses is generated server-side by a pure lookup:

```java
// NextStepService.java
public String getPrompt(ApplicationStatus status) {
    return switch (status) {
        case IDENTIFIED        -> "Review the job description and assess fit before moving forward.";
        case REVIEWED          -> "Tailor your resume to highlight relevant experience for this role.";
        case CUSTOMIZED_RESUME -> "Submit your application. Don't wait — roles fill fast.";
        case SUBMITTED         -> "Set a follow-up reminder for 5–7 business days if no response.";
        case FIRST_CONTACT     -> "Respond promptly. Schedule the recruiter screen at your earliest availability.";
        case RECRUITER_SCREEN  -> "Prepare role-specific talking points. Research the company's engineering blog.";
        case TECH_INTERVIEW    -> "Confirm logistics. Practice relevant coding patterns and system design.";
        case OFFER             -> "Review the full offer. Compare total compensation, not just salary.";
        case CLOSED            -> "Archive this one. Reflect on what worked for your next application.";
        case REJECTED          -> "Note what stage you reached. Every process builds interview muscle.";
    };
}
```

---

## 8. Project Repository Structure

```
job-tracker/                          (monorepo root)
├── backend/                          (Spring Boot API)
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/jobtracker/
│   │   │   │   └── [packages per §6]
│   │   │   └── resources/
│   │   │       ├── application.yml
│   │   │       ├── application-dev.yml
│   │   │       └── db/migration/    (Flyway V1–V5)
│   │   └── test/
│   │       └── java/com/jobtracker/
│   ├── Dockerfile
│   └── pom.xml
├── frontend/                         (Flutter Web)
│   ├── lib/                          [structure per §6]
│   ├── web/
│   │   └── index.html
│   ├── test/
│   ├── Dockerfile                    (nginx-based build+serve)
│   └── pubspec.yaml
├── nginx/
│   ├── nginx.conf
│   └── certs/                        (.gitignored)
├── docker-compose.yml                (VPS production)
├── docker-compose.dev.yml            (local dev)
├── .env.example                      (all required env vars documented)
└── README.md
```

---

## 9. Open Question Resolutions

All 6 open questions from the PRD are resolved:

| OQ | Resolution | ADR |
|----|-----------|-----|
| OQ-001: JWT refresh strategy | Access token 15 min + refresh cookie 7 days; silent refresh via Dio interceptor | ADR-003 |
| OQ-002: SMTP provider | Postmark (free tier, SMTP-agnostic config) | ADR-006 |
| OQ-003: URL parse fallback UX | Partial pre-fill + inline "enter manually" message; never blocks the form | ADR-007 |
| OQ-004: Stale notification frequency | Every 7 days until action taken or opt-out | ADR-008 |
| OQ-005: Dashboard default sort | `last_status_changed_at` DESC | ADR-009 |
| OQ-006: Layout strategy | Desktop-first; responsive breakpoints at 960px / 600px | ADR-010 |

---

## 10. Next Steps

- [ ] **Epic & Story Elaboration** — Activate `*sm`; expand E-001–E-008 story headings into full story files with acceptance criteria, referencing this document for implementation constraints
- [ ] **Implementation Readiness Check** — Activate `*architect`; run `IR` skill to validate PRD ↔ Architecture ↔ Epics alignment before first sprint
- [ ] **Backend scaffold** — Create `job-tracker/backend` repo, Spring Boot project with Flyway baseline (V1 schema migration)
- [ ] **Frontend scaffold** — Create `job-tracker/frontend` Flutter Web project with go_router + BLoC skeleton
- [ ] **Infrastructure** — Create Docker Compose dev stack; confirm VPS provider and domain

---

_Generated by Winston (BMad Architect Agent) · job-tracker-planning · 2026-03-27_
_Input: PRD v1.0, Product Brief, Platform Requirements, Trigger Map, Design Deliveries DD-001_

