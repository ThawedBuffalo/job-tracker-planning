---
stepsCompleted:
  - step-01-validate-prerequisites
  - step-02-design-epics
  - step-03-create-stories
  - step-04-final-validation
inputDocuments:
  - _bmad-output/planning-artifacts/prd.md
  - _bmad-output/planning-artifacts/architecture.md
  - design-artifacts/A-Product-Brief/project-brief.md
  - design-artifacts/E-PRD/Design-Deliveries/DD-001-application-capture-and-follow-up.yaml
status: Legacy Reference (Monolith Baseline)
supersededBy:
  - _bmad-output/planning-artifacts/architecture-polyrepo.md
  - _bmad-output/planning-artifacts/STORY-MAP.md
---

# job-tracker — Epic & Story Breakdown

> Legacy artifact from monolith planning pass. Keep for reference only.
> Active execution sequencing and dependency source of truth: `_bmad-output/planning-artifacts/STORY-MAP.md`.

**Author:** JC
**SM Agent:** Bob
**Date:** 2026-03-27
**Version:** 1.0

---

## Overview

Complete epic and story breakdown for `job-tracker`, decomposing PRD functional requirements and architecture constraints into dev-agent-ready implementation stories.

**Stack:** Flutter Web (frontend) · Spring Boot 3.4 / Java 21 (backend API) · PostgreSQL 16 · Docker Compose + Nginx (infrastructure)

**Architectural note (legacy):** This document assumes a **monolith** baseline and is not the active implementation source after polyrepo re-baselining.

---

## Requirements Inventory

### Functional Requirements

| ID | Area | Requirement |
|----|------|-------------|
| FR-001 | Auth | Visitor can register with email and password |
| FR-002 | Auth | Registered user can sign in with email and password |
| FR-003 | Auth | System issues short-lived JWT on successful auth |
| FR-004 | Auth | Authenticated user can sign out |
| FR-005 | Auth | Authenticated user can update email or password |
| FR-006 | Auth | Passwords stored using BCrypt/Argon2; no plaintext |
| FR-010 | Capture | User can create application manually with required fields |
| FR-011 | Capture | User can paste job URL to initiate capture |
| FR-012 | Capture | System parses URL to extract job title, company, URL |
| FR-013 | Capture | User can edit auto-populated fields before saving |
| FR-014 | Capture | System blocks save and highlights missing required fields |
| FR-015 | Capture | New application defaults to `identified` status |
| FR-016 | Capture | User can attach optional fields to any application |
| FR-017 | Capture | User can edit existing application record |
| FR-018 | Capture | User can delete application record |
| FR-020 | Pipeline | User can advance application to next pipeline stage |
| FR-021 | Pipeline | User can manually set application to any non-terminal stage |
| FR-022 | Pipeline | User can mark application as `rejected` |
| FR-023 | Pipeline | User can mark application as `closed` |
| FR-024 | Pipeline | Terminal statuses require explicit intent to reverse |
| FR-025 | Pipeline | Every status change recorded as append-only audit event with timestamp |
| FR-026 | Pipeline | System displays context-sensitive next-step prompt after every status change |
| FR-030 | Dashboard | User can view dashboard listing all active applications with status |
| FR-031 | Dashboard | Dashboard groups/sorts by status and recency |
| FR-032 | Dashboard | Dashboard visually distinguishes stalled applications (7+ days) |
| FR-033 | Dashboard | User can view full detail record of any application |
| FR-034 | Dashboard | User can view complete chronological status history |
| FR-035 | Dashboard | User can filter applications by status |
| FR-036 | Dashboard | User can search by company name or job title |
| FR-040 | Notifications | System sends email on status change |
| FR-041 | Notifications | System sends email on follow-up date reached |
| FR-042 | Notifications | System sends email when application stalled 7 days |
| FR-043 | Notifications | Stall notifications only sent for non-terminal applications |
| FR-044 | Notifications | Email includes: name, company, status, direct link |
| FR-045 | Notifications | User can opt out of individual notification types |
| FR-046 | Notifications | System retries failed sends with exponential backoff |
| FR-050 | Integrity | All authenticated endpoints require valid JWT |
| FR-051 | Integrity | Users can only access their own records |
| FR-052 | Integrity | System logs security-relevant events |
| FR-053 | Integrity | Records carry created_at / updated_at timestamps |
| FR-054 | Integrity | Status events are immutable once recorded |

### Non-Functional Requirements

| ID | Area | Target |
|----|------|--------|
| NFR-P-001 | Performance | Dashboard first load < 3s on 4G |
| NFR-P-002 | Performance | API response p95 < 500ms |
| NFR-P-003 | Performance | URL parse + pre-fill < 2s |
| NFR-R-001 | Reliability | 99.5% uptime |
| NFR-R-002 | Reliability | Email retry up to 24h on failure |
| NFR-R-003 | Reliability | Daily automated DB backup, 7-day retention |
| NFR-S-001 | Security | BCrypt cost ≥ 12 |
| NFR-S-002 | Security | JWT ≤ 15 min access token |
| NFR-S-003 | Security | HTTPS enforced via Nginx TLS |
| NFR-S-004 | Security | Security headers: HSTS, CSP, X-Frame-Options, etc. |
| NFR-S-005 | Security | SMTP credentials in env vars only |
| NFR-S-006 | Security | All user data scoped by user_id |
| NFR-M-001 | Maintainability | DB changes via Flyway migrations |
| NFR-M-002 | Maintainability | Docker Compose dev/prod parity |
| NFR-M-003 | Maintainability | All secrets via environment variables |
| NFR-M-004 | Maintainability | API errors return structured JSON |
| NFR-A-001 | Accessibility | Responsive: desktop + mobile browsers |
| NFR-A-005 | Accessibility | WCAG 2.1 AA on core flows |

### Additional Requirements (Architecture)

- ADR-001: Single-process monolith
- ADR-003: JWT refresh token (15 min access + 7-day refresh cookie)
- ADR-007: Jsoup server-side URL parsing; partial-fail returns available fields
- ADR-008: Stale notification repeats every 7 days until action or opt-out
- ADR-009: Dashboard default sort: `last_status_changed_at` DESC
- ADR-010: Desktop-first Flutter layout; breakpoints 960px / 600px

### FR Coverage Map

| Epic | Stories | FRs Covered |
|------|---------|-------------|
| E-001 Auth & Account | 1.1–1.5 | FR-001–006, FR-050, FR-052 |
| E-002 Application Capture | 2.1–2.5 | FR-010, FR-014–018, FR-053 |
| E-003 URL Intake & Parsing | 3.1–3.4 | FR-011–013 |
| E-004 Pipeline Management | 4.1–4.5 | FR-020–026 |
| E-005 Dashboard & Visibility | 5.1–5.5 | FR-030–036 |
| E-006 Email Notifications | 6.1–6.5 | FR-040–046 |
| E-007 Data Integrity & Audit | 7.1–7.2 | FR-051, FR-054, NFR-S-006 |
| E-008 Infrastructure & DevOps | 8.1–8.4 | NFR-M-001–003, NFR-R-003, NFR-S-003–004 |

---

## Epic List

| # | Epic | Priority | Story Count |
|---|------|----------|-------------|
| E-001 | Auth & Account | P0 | 5 |
| E-002 | Application Capture | P0 | 5 |
| E-003 | URL Intake & Parsing | P0 | 4 |
| E-004 | Pipeline Management | P0 | 5 |
| E-005 | Dashboard & Visibility | P0 | 5 |
| E-006 | Email Notifications | P0 | 5 |
| E-007 | Data Integrity & Audit | P0 | 2 |
| E-008 | Infrastructure & DevOps | P0 | 4 |

**Total: 35 stories**

**Recommended implementation sequence:** E-008 → E-001 → E-002 → E-003 → E-004 → E-005 → E-006 → E-007

---

## Epic 1: Auth & Account

**Goal:** Allow users to register, sign in with a JWT-based session, sign out, and manage their account — forming the security foundation for all other features.

**FRs:** FR-001–006, FR-050, FR-052
**NFRs:** NFR-S-001, NFR-S-002, NFR-S-004

---

### Story 1.1: User Registration

As a **visitor**,
I want to create an account with my email and password,
So that I can access my personal job-tracking workspace.

**Acceptance Criteria:**

**Given** I am on the Register page (`/register`)
**When** I submit a valid email and password (≥ 8 chars, at least 1 number)
**Then** the API creates a new `users` record with a BCrypt-hashed password (cost 12)
**And** the API returns HTTP 201 with `{ id, email, created_at }`
**And** the Flutter UI navigates me to the Login page with a success toast "Account created — sign in to get started"

**Given** I submit an email that is already registered
**When** the API processes the request
**Then** the API returns HTTP 409 with `{ "error": "EMAIL_ALREADY_REGISTERED" }`
**And** the Flutter UI displays an inline error: "An account with this email already exists"

**Given** I submit a password shorter than 8 characters
**When** the API validates the request
**Then** the API returns HTTP 400 with `{ "error": "VALIDATION_ERROR", "field": "password" }`
**And** the Flutter UI highlights the password field with "Must be at least 8 characters"

**Given** I submit an invalid email format
**When** the API validates the request
**Then** the API returns HTTP 400 before any DB write
**And** the Flutter UI highlights the email field with "Enter a valid email address"

**Implementation notes:**
- Backend: `POST /api/v1/auth/register` → `AuthController` → `AuthService.register()`
- Password: `BCryptPasswordEncoder(12)` via Spring Security
- No email verification in v1
- Flutter: `RegisterPage` + `AuthBloc(RegisterSubmitted)` → `AuthRepository.register()`

---

### Story 1.2: User Sign-In with JWT Session

As a **registered user**,
I want to sign in with my email and password,
So that I receive a secure session and can access my applications.

**Acceptance Criteria:**

**Given** I am on the Login page (`/login`)
**When** I submit correct email and password
**Then** the API returns HTTP 200 with `{ "access_token": "eyJ...", "expires_in": 900 }`
**And** the API sets an HttpOnly Secure cookie: `refresh_token` (Max-Age: 604800, Path: `/api/v1/auth/refresh`)
**And** the Flutter app stores the access token in `flutter_secure_storage`
**And** the Flutter app navigates to `/dashboard`

**Given** I submit an incorrect password or unregistered email
**When** the API processes the request
**Then** the API returns HTTP 401 with `{ "error": "INVALID_CREDENTIALS" }`
**And** the Flutter UI displays "Email or password is incorrect" (no field-level differentiation, prevents user enumeration)

**Given** my access token is within 60 seconds of expiry
**When** I make any API request via the Dio `AuthInterceptor`
**Then** the interceptor automatically calls `POST /api/v1/auth/refresh` with the refresh cookie
**And** a new access token is stored and the original request is retried transparently

**Given** my refresh token has expired
**When** the interceptor attempts silent refresh
**Then** the API returns HTTP 401 from the refresh endpoint
**And** the Flutter app dispatches `SessionExpired` to `AuthBloc`
**And** the user is redirected to `/login` with toast "Your session expired — please sign in again"

**Implementation notes:**
- Backend: `POST /api/v1/auth/login` → `JwtService.generateAccessToken()` + `JwtService.generateRefreshToken()`
- Refresh cookie: `ResponseCookie.from("refresh_token", token).httpOnly(true).secure(true).sameSite("Strict").path("/api/v1/auth/refresh").maxAge(604800).build()`
- Flutter: `AuthInterceptor` on `Dio` instance; `flutter_secure_storage` for access token

---

### Story 1.3: User Sign-Out

As an **authenticated user**,
I want to sign out of my account,
So that my session is terminated and my data is no longer accessible.

**Acceptance Criteria:**

**Given** I am authenticated and click "Sign Out"
**When** the Flutter app calls `POST /api/v1/auth/logout`
**Then** the API clears the `refresh_token` cookie (Max-Age: 0)
**And** the API returns HTTP 200
**And** the Flutter app deletes the access token from `flutter_secure_storage`
**And** the Flutter app navigates to `/login`
**And** all subsequent API calls from the Flutter app return 401 until re-authentication

**Given** I am on any protected route when my access token becomes invalid
**When** the Dio interceptor cannot refresh (no valid cookie)
**Then** the app clears stored credentials and redirects to `/login`

**Implementation notes:**
- Backend: `POST /api/v1/auth/logout` → clear cookie in response
- Flutter: `AuthBloc(SignOutRequested)` → `AuthRepository.logout()` → delete storage → navigate `/login`
- `go_router` redirect guard: unauthenticated → `/login`

---

### Story 1.4: Password Change

As an **authenticated user**,
I want to change my password,
So that I can maintain account security.

**Acceptance Criteria:**

**Given** I navigate to Settings (`/settings`) and submit my current password plus a new password
**When** the API validates the request
**Then** the API verifies current password against the stored BCrypt hash
**And** if valid, stores the new BCrypt hash and returns HTTP 200
**And** the Flutter UI displays "Password updated successfully"

**Given** I submit an incorrect current password
**When** the API processes the request
**Then** the API returns HTTP 401 with `{ "error": "INVALID_CURRENT_PASSWORD" }`
**And** the Flutter UI displays "Current password is incorrect"

**Given** I submit a new password shorter than 8 characters
**When** the API validates the request
**Then** the API returns HTTP 400 before any DB write

**Implementation notes:**
- Backend: `PUT /api/v1/users/me` with `{ "current_password", "new_password" }`
- All existing refresh tokens remain valid after password change (acceptable v1 trade-off; force re-login post-v1)
- Flutter: Settings page with `ChangePasswordForm` widget

---

### Story 1.5: Route Protection & Security Headers

As a **developer**,
I want all authenticated routes protected and security headers enforced,
So that unauthenticated users cannot access private data and the app is hardened against common web attacks.

**Acceptance Criteria:**

**Given** a request arrives at any `/api/v1/` endpoint (except `/auth/register`, `/auth/login`, `/auth/refresh`)
**When** no valid `Authorization: Bearer` header is present
**Then** the API returns HTTP 401 with `{ "error": "UNAUTHORIZED" }`

**Given** a valid JWT is presented but its `user_id` does not match the resource owner
**When** the API resolves the resource
**Then** the API returns HTTP 403 with `{ "error": "FORBIDDEN" }`

**Given** any HTTP response is sent through Nginx
**When** the response headers are inspected
**Then** the following headers are present:
- `Strict-Transport-Security: max-age=31536000; includeSubDomains`
- `X-Frame-Options: DENY`
- `X-Content-Type-Options: nosniff`
- `Content-Security-Policy: default-src 'self'; ...`
- `Referrer-Policy: strict-origin-when-cross-origin`

**Given** all HTTP traffic arrives at port 80
**When** Nginx processes the request
**Then** a 301 redirect to HTTPS is issued

**Implementation notes:**
- Backend: `SecurityConfig.java` — `JwtAuthFilter` in filter chain; permit `/auth/**`
- Nginx: security headers in `nginx.conf` (see Architecture Doc §6)
- Flutter: `go_router` redirect: if `!authBloc.isAuthenticated` → `/login`

---

## Epic 2: Application Capture

**Goal:** Allow users to create, edit, and delete application records manually, with required-field enforcement preventing incomplete saves.

**FRs:** FR-010, FR-014–018, FR-053
**NFRs:** NFR-M-004

---

### Story 2.1: Create Application (Manual Entry)

As an **authenticated user**,
I want to create a new job application by entering the required fields,
So that I can begin tracking a new opportunity in my pipeline.

**Acceptance Criteria:**

**Given** I click "Add Application" on the Dashboard and choose manual entry
**When** I fill in `job_title`, `company_name`, and `job_url` and submit
**Then** the API creates an `applications` record with `status = 'identified'` and returns HTTP 201
**And** the API response includes the full application object with `id`, `created_at`, `updated_at`
**And** the Flutter UI navigates to the Application Detail page for the new record
**And** the Dashboard list updates to include the new application at the top

**Given** I submit without filling `job_title`
**When** the API validates the payload
**Then** the API returns HTTP 400 with `{ "error": "VALIDATION_ERROR", "field": "job_title" }`
**And** the Flutter form highlights `job_title` with "Required"

**Given** I submit without filling `company_name`
**Then** the same validation applies for `company_name`

**Given** I submit without filling `job_url`
**Then** the same validation applies for `job_url`

**Implementation notes:**
- Backend: `POST /api/v1/applications` → `ApplicationController` → `ApplicationService.create()`
- DB: INSERT into `applications`; INSERT initial event into `application_events` (`from_status = NULL`, `to_status = 'identified'`)
- Flutter: `AddApplicationPage` (manual mode) + `ApplicationFormBloc`
- `job_url` validation: must be a valid URL format (regex or `URI.parse`)

---

### Story 2.2: Required-Field Validation Gate

As an **authenticated user**,
I want the form to prevent saving until all required fields are filled,
So that I never create an incomplete application record.

**Acceptance Criteria:**

**Given** I am on the Add Application form
**When** I attempt to submit with one or more required fields empty
**Then** all empty required fields are outlined in red with "Required" labels simultaneously
**And** the Submit button remains disabled until all required fields have content
**And** no API call is made while required fields are empty

**Given** I clear a previously filled required field
**When** focus leaves the field
**Then** the field immediately shows the "Required" error state without re-submission

**Given** all required fields have valid content
**When** I view the Submit button
**Then** the Submit button is enabled

**Implementation notes:**
- Flutter: `ApplicationFormBloc` holds a `FormStatus` state; `isValid` computed from required fields
- Primary validation is client-side (reactive); backend validation is a safety net (Story 2.1)
- `TextFormField` with `validator` + `autovalidateMode: AutovalidateMode.onUserInteraction`

---

### Story 2.3: Edit Application Record

As an **authenticated user**,
I want to edit any field on an existing application,
So that I can correct mistakes or add information as my search progresses.

**Acceptance Criteria:**

**Given** I am on the Application Detail page
**When** I click "Edit" and change `job_title` and `notes` and save
**Then** the API updates the `applications` record and returns HTTP 200 with the full updated object
**And** `updated_at` is refreshed in the response
**And** the Flutter UI reflects the updated values immediately without a full page reload

**Given** I edit a required field to be empty and attempt to save
**Then** the same required-field validation gate from Story 2.2 applies

**Given** I attempt to edit an application owned by another user (direct API call)
**When** the API resolves ownership
**Then** the API returns HTTP 403

**Implementation notes:**
- Backend: `PUT /api/v1/applications/{id}` — partial update (only fields provided in body are changed)
- Flutter: Edit mode in `ApplicationDetailPage`; pre-populated form fields; `ApplicationDetailBloc(EditSubmitted)`

---

### Story 2.4: Delete Application

As an **authenticated user**,
I want to delete an application record,
So that I can remove positions I'm no longer tracking.

**Acceptance Criteria:**

**Given** I am on the Application Detail page
**When** I click "Delete" and confirm the deletion dialog
**Then** the API deletes the `applications` record (cascade deletes `application_events` and `notification_log` rows)
**And** the API returns HTTP 204
**And** the Flutter UI navigates back to the Dashboard
**And** the deleted application no longer appears in the Dashboard list

**Given** I click "Delete" but dismiss the confirmation dialog
**Then** no API call is made and the application record is unchanged

**Given** I attempt to delete an application owned by another user (direct API call)
**Then** the API returns HTTP 403

**Implementation notes:**
- Backend: `DELETE /api/v1/applications/{id}` with ownership check
- Flutter: `ConfirmDeleteDialog` widget; `ApplicationDetailBloc(DeleteConfirmed)`
- ON DELETE CASCADE in schema handles child records

---

### Story 2.5: Manage Optional Fields

As an **authenticated user**,
I want to add and edit optional fields on an application (notes, job description, salary, contact, dates),
So that I can store the context I need for interview prep and follow-up.

**Acceptance Criteria:**

**Given** I am on the Application Detail page in edit mode
**When** I add a value to `notes`, `salary_range`, `contact_name`, `contact_email`, `applied_date`, and `follow_up_date` and save
**Then** the API stores all optional fields against the application record
**And** the Application Detail page displays all non-empty optional fields

**Given** I set a `follow_up_date` to a past date
**When** the API accepts the value
**Then** the API stores it without validation error (follow-up overdue handling is the notification job's responsibility)

**Given** `contact_email` is provided
**When** the API validates the field
**Then** the API validates it as a valid email format; returns HTTP 400 if invalid

**Given** I clear a previously set optional field and save
**Then** the API stores `null` for that field and the detail page no longer shows it

**Implementation notes:**
- Backend: Handled by same `PUT /api/v1/applications/{id}` as Story 2.3 (same endpoint, additional fields)
- Flutter: Optional field section in `ApplicationDetailPage`; collapsed by default, expandable
- `applied_date` and `follow_up_date` use Flutter `DatePicker`

---

## Epic 3: URL Intake & Parsing

**Goal:** Reduce application entry friction by parsing a pasted job URL server-side and pre-filling form fields, with graceful fallback to manual entry when parsing fails.

**FRs:** FR-011–013
**NFRs:** NFR-P-003

---

### Story 3.1: URL Paste Intake Flow (UI)

As an **authenticated user**,
I want to paste a job URL to start adding a new application,
So that I spend less time typing and more time applying.

**Acceptance Criteria:**

**Given** I click "Add Application" on the Dashboard
**When** the Add Application modal/page opens
**Then** I see two entry options: "Paste job URL" (primary) and "Enter manually" (secondary)

**Given** I select "Paste job URL" and paste a URL into the field
**When** I press the "Parse" button (or focus leaves the field)
**Then** the Flutter app calls `POST /api/v1/parse-url` with `{ "url": "..." }` and shows a loading indicator

**Given** the parse returns successfully
**When** results are received
**Then** the form fields (`job_title`, `company_name`, `job_url`) are pre-populated with parsed values
**And** I can see and edit all pre-populated fields before saving

**Given** the parse takes longer than 2 seconds
**When** the loading indicator is visible
**Then** the user can cancel the parse and switch to manual entry without losing the URL

**Implementation notes:**
- Flutter: `AddApplicationPage` with toggle between URL-paste and manual entry modes
- `UrlPasteField` widget handles paste detection and API call trigger
- `ApplicationFormBloc(UrlParsed(response))` populates form state

---

### Story 3.2: Backend URL Parse Endpoint

As a **developer**,
I want a server-side URL parsing endpoint,
So that job page content is fetched and parsed without CORS restrictions.

**Acceptance Criteria:**

**Given** I call `POST /api/v1/parse-url` with `{ "url": "https://jobs.acme.com/123" }`
**When** Jsoup fetches and parses the page
**Then** the API extracts: page `<title>`, OpenGraph `og:title`, `og:site_name`
**And** returns HTTP 200 with `{ "job_title": "...", "company_name": "...", "job_url": "..." }`

**Given** the page has an OpenGraph `og:title` tag
**When** both `<title>` and `og:title` are available
**Then** `og:title` is preferred for `job_title` (usually cleaner than full page title)

**Given** Jsoup can fetch the page but `company_name` cannot be detected
**When** the API builds the response
**Then** the API returns `"company_name": null` and `"parse_warnings": ["company_name could not be detected"]`
**And** HTTP status is still 200 (partial success is not an error)

**Given** the URL is unreachable (timeout, DNS failure, 4xx/5xx response)
**When** Jsoup throws an exception
**Then** the API returns HTTP 200 with all fields null and `"parse_warnings": ["Could not read that URL. Enter details manually."]`
**And** the API does NOT return HTTP 500 or expose stack traces

**Given** the URL is malformed (not a valid URL)
**When** the API validates the request
**Then** the API returns HTTP 400 with `{ "error": "VALIDATION_ERROR", "field": "url" }`

**Implementation notes:**
- Backend: `POST /api/v1/parse-url` → `UrlParseController` → `UrlParseService.parse(url)`
- `UrlParseService`: use `Jsoup.connect(url).timeout(5000).get()`; wrap in try/catch
- Set a User-Agent header to avoid bot-blocking: `"Mozilla/5.0 (compatible; job-tracker/1.0)"`
- Jsoup 1.18.x dependency in `pom.xml`

---

### Story 3.3: Parsed Fields Pre-Fill & Editing

As an **authenticated user**,
I want to review and edit all parsed fields before saving my application,
So that I can correct any parsing errors before they enter my records.

**Acceptance Criteria:**

**Given** the URL parse returns with values
**When** the form is populated
**Then** all parsed fields are fully editable (no read-only parsed fields)
**And** a subtle "Auto-filled" label appears next to each pre-populated field

**Given** I edit a pre-populated field
**When** I clear the "Auto-filled" indicator by editing
**Then** the label disappears and the field behaves identically to a manually typed field

**Given** `job_title` is pre-filled but I clear it and attempt to save
**Then** the required-field validation gate (Story 2.2) applies; save is blocked

**Given** parse returned `company_name: null`
**When** the form is shown
**Then** the `company_name` field is empty with placeholder "Could not detect — enter manually"
**And** it is highlighted as a required field needing completion

**Implementation notes:**
- Flutter: `ApplicationFormBloc` state holds `isParsed: bool` flag per field; used for "Auto-filled" label display
- No special API behavior; saving a parsed + edited form uses the same `POST /api/v1/applications` as Story 2.1

---

### Story 3.4: Parse Failure Graceful Fallback

As an **authenticated user**,
I want the app to gracefully handle URL parsing failures,
So that I can always add an application even if the URL cannot be parsed.

**Acceptance Criteria:**

**Given** the parse API returns all fields as null (full failure)
**When** the Flutter UI receives the response
**Then** a non-blocking toast is shown: "Couldn't read that URL. Fill in the details manually."
**And** the form opens in manual entry mode with the URL pre-filled in `job_url`
**And** `job_title` and `company_name` are empty and highlighted as required

**Given** the parse API call times out on the client side (> 8 seconds)
**When** the Dio request times out
**Then** the Flutter UI shows the same fallback toast and switches to manual entry mode
**And** the user does NOT see an unhandled error dialog

**Given** the user is offline when they attempt to parse a URL
**When** Dio throws a connectivity error
**Then** the Flutter UI shows: "No connection — enter details manually" and opens manual entry mode

**Implementation notes:**
- Flutter: `ApplicationFormBloc` handles `UrlParseError` state; maps all error cases to manual entry mode
- Dio client timeout: `connectTimeout: 8s`, `receiveTimeout: 8s`

---

## Epic 4: Pipeline Management

**Goal:** Enable users to move applications through the canonical 9-stage pipeline with guided next-step prompts, full override capability, and terminal status marking.

**FRs:** FR-020–026
**NFRs:** NFR-M-004

---

### Story 4.1: Advance Application Status

As an **authenticated user**,
I want to advance my application to the next stage in the pipeline,
So that my pipeline reflects the true current state of each opportunity.

**Acceptance Criteria:**

**Given** I am on the Application Detail page for an application with status `submitted`
**When** I click "Move to Next Stage" (or equivalent primary action button)
**Then** the Flutter UI calls `POST /api/v1/applications/{id}/status` with `{ "status": "first_contact" }`
**And** the API updates `applications.status` to `first_contact`
**And** the API sets `applications.last_status_changed_at` to NOW()
**And** the API inserts a new row in `application_events` with `from_status = 'submitted'`, `to_status = 'first_contact'`
**And** the API returns HTTP 200 with the updated application and `next_step_prompt`

**Given** the application is at the last non-terminal status (`offer`)
**When** I view the action controls
**Then** the "Move to Next Stage" button is replaced by "Mark as Closed" and "Mark as Rejected" buttons only

**Given** the status change succeeds
**When** the Flutter UI renders the response
**Then** the status chip on the detail page updates immediately
**And** a `NextStepBanner` widget displays the `next_step_prompt` from the API response

**Implementation notes:**
- Backend: `ApplicationService.advanceStatus()` — validate ownership, record event, update status, call `NextStepService.getPrompt()`
- Flutter: `ApplicationDetailBloc(AdvanceStatusRequested)` → `ApplicationRepository.advanceStatus()`
- Pipeline order: identified → reviewed → customized_resume → submitted → first_contact → recruiter_screen → tech_interview → offer

---

### Story 4.2: Manual Status Override

As an **authenticated user**,
I want to set my application to any non-terminal stage directly,
So that I can correct mistakes or handle out-of-order employer responses.

**Acceptance Criteria:**

**Given** I am on the Application Detail page
**When** I open the status selector dropdown and choose `recruiter_screen` (from `identified`)
**Then** the Flutter UI calls `POST /api/v1/applications/{id}/status` with `{ "status": "recruiter_screen" }`
**And** the API accepts the status change regardless of the gap between current and target status
**And** the API records the transition event with `from_status = 'identified'`, `to_status = 'recruiter_screen'`

**Given** I attempt to set status to `closed` or `rejected` via the standard status selector
**When** the Flutter UI renders the dropdown
**Then** `closed` and `rejected` are NOT available in the standard status selector (use dedicated buttons per Story 4.3/4.4)

**Given** I supply a `note` with the status change (optional)
**When** the API records the event
**Then** `application_events.note` stores the supplied text

**Implementation notes:**
- Flutter: `StatusSelectorDropdown` widget listing all 9 non-terminal statuses; separate "Mark as Rejected / Closed" section
- Backend: `StatusChangeRequest` DTO contains `status` (enum) + optional `note`; no sequential-order validation required

---

### Story 4.3: Mark Application as Rejected or Closed

As an **authenticated user**,
I want to mark an application as rejected or closed,
So that I can accurately reflect outcomes and keep my active pipeline clean.

**Acceptance Criteria:**

**Given** I am on the Application Detail page
**When** I click "Rejected by employer" and confirm
**Then** the API sets status to `rejected`, records the event, and returns HTTP 200
**And** the application card on the Dashboard moves to a "Closed / Rejected" section (or is filtered out of active view)

**Given** I click "Withdraw / Close" and confirm
**When** the API processes the request
**Then** the API sets status to `closed`, records the event, and returns HTTP 200

**Given** an application is in `rejected` or `closed` status
**When** I view the Application Detail page
**Then** the status controls show a "Reopen" option with a warning: "This will move the application back to Reviewed. Continue?"
**And** on confirmation, the API accepts the status change to `reviewed` (override of terminal status with explicit intent per FR-024)

**Implementation notes:**
- Flutter: Two clearly labelled destructive-action buttons with `ConfirmDialog` on each
- Backend: No special logic needed — same `POST /api/v1/applications/{id}/status` endpoint; `rejected` and `closed` are valid enum values
- `Reopen` calls same endpoint with `{ "status": "reviewed" }` + confirmation UX

---

### Story 4.4: Status Audit History

As an **authenticated user**,
I want to see the full history of status changes for an application,
So that I can trace the timeline of each opportunity and learn from the journey.

**Acceptance Criteria:**

**Given** I am on the Application Detail page
**When** I scroll to the "History" section
**Then** I see a chronological list of all status transitions in descending order (most recent first)
**And** each event shows: `from_status` → `to_status`, timestamp (formatted as "Mar 25, 2026 at 2:15 PM"), and `note` if present

**Given** the application has only been created (one event: `null → identified`)
**When** I view the History section
**Then** I see one entry: "Added to pipeline" with the creation timestamp

**Given** I call `GET /api/v1/applications/{id}/events`
**When** the API resolves ownership
**Then** the API returns the event list ordered by `created_at DESC`
**And** events from other users' applications return HTTP 403

**Implementation notes:**
- Backend: `GET /api/v1/applications/{id}/events` → `ApplicationEventRepository.findByApplicationIdOrderByCreatedAtDesc()`
- Flutter: `StatusTimeline` widget in `ApplicationDetailPage`; uses `ApplicationDetailBloc` state

---

### Story 4.5: Next-Step Guidance Prompt

As an **authenticated user**,
I want to see a contextual "what to do next" prompt after every status change,
So that I always know the recommended next action without having to think about it.

**Acceptance Criteria:**

**Given** any status change is successfully saved (via advance, override, or terminal marking)
**When** the API returns the updated application
**Then** the response includes `next_step_prompt` — a non-empty string specific to the new status

**Given** the new status is `submitted`
**Then** `next_step_prompt` is "Set a follow-up reminder for 5–7 business days if no response."

**Given** the new status is `recruiter_screen`
**Then** `next_step_prompt` is "Prepare role-specific talking points. Research the company's engineering blog."

**Given** any of the 10 statuses is set
**Then** a unique, non-generic prompt is returned (no "Good luck!" or "Keep going!" filler)

**Given** the Flutter UI receives the updated application with `next_step_prompt`
**When** the Application Detail page renders
**Then** a `NextStepBanner` widget is prominently displayed below the status chip
**And** the banner is dismissible (dismissed state is local/ephemeral; reappears on next status change)

**Implementation notes:**
- Backend: `NextStepService.java` — `switch` on `ApplicationStatus` returning pre-defined strings (see Architecture Doc §7)
- All 10 status prompts must be defined; no null/empty returns
- Flutter: `NextStepBanner` widget; dismissed via local state (not persisted)

---

## Epic 5: Dashboard & Visibility

**Goal:** Give users a clear, scannable overview of their entire pipeline with fast access to any application, filtering, search, and stale-application highlighting.

**FRs:** FR-030–036
**NFRs:** NFR-P-001, NFR-A-001

---

### Story 5.1: Dashboard — Application List

As an **authenticated user**,
I want to see all my active applications on a dashboard,
So that I can get a quick overview of my pipeline at any time.

**Acceptance Criteria:**

**Given** I navigate to `/dashboard`
**When** the page loads
**Then** `GET /api/v1/applications` is called with default params (sort: `last_status_changed_at` DESC, page: 0, size: 20)
**And** each application card displays: job title, company name, status chip (color-coded by status), and relative time since last status change ("3 days ago")

**Given** I have 0 active applications
**When** the dashboard loads
**Then** an empty state is shown: "No active applications yet. Add one to start your pipeline." with an "Add Application" CTA button

**Given** the API returns `total_pages > 1`
**When** I scroll to the bottom of the list
**Then** the next page is loaded and appended (infinite scroll or "Load more" button)

**Given** the dashboard first meaningful render time
**When** measured on a 4G connection
**Then** application cards are visible within 3 seconds (NFR-P-001)

**Implementation notes:**
- Flutter: `DashboardPage` + `DashboardBloc(LoadApplications)` → `ApplicationRepository.list()`
- Status chip colors: define in `AppColors` — one distinct color per status group (applied/interviewing/terminal)
- Relative time: use `timeago` package

---

### Story 5.2: Stale Application Indicator

As an **authenticated user**,
I want stalled applications to be visually flagged on the dashboard,
So that I can take action before opportunities slip away.

**Acceptance Criteria:**

**Given** an application's `last_status_changed_at` is more than 7 days ago
**When** the dashboard renders the application card
**Then** a "Stale" badge or warning icon is displayed on the card
**And** the card has a distinct visual treatment (amber border, warning icon) setting it apart from active cards

**Given** the API response includes `"is_stale": true` on the application object
**When** the Flutter `ApplicationCard` widget renders
**Then** it conditionally renders the `StaleBadge` widget

**Given** an application's `last_status_changed_at` is updated today (user takes action)
**When** the dashboard refreshes
**Then** the `StaleBadge` is no longer shown for that application

**Implementation notes:**
- Backend: `ApplicationResponse` includes `is_stale: boolean` = `lastStatusChangedAt < NOW() - 7 days && status NOT IN (closed, rejected)`
- Flutter: `StaleBadge` widget (amber chip with warning icon); rendered conditionally in `ApplicationCard`

---

### Story 5.3: Filter and Sort Applications

As an **authenticated user**,
I want to filter my applications by status and sort them,
So that I can focus on specific pipeline stages.

**Acceptance Criteria:**

**Given** I am on the Dashboard
**When** I select "Submitted" from the status filter chips
**Then** the Flutter app calls `GET /api/v1/applications?status=submitted`
**And** only applications with status `submitted` are displayed

**Given** I select "All" from the filter
**When** the request is sent
**Then** no `status` query param is included and all active applications are shown

**Given** I select sort order "Date Added" (vs default "Last Activity")
**When** the request is sent
**Then** `GET /api/v1/applications?sort=created_at&order=desc` is called

**Given** I combine a status filter with a sort order
**When** the request is sent
**Then** both `status` and `sort` query params are included together

**Implementation notes:**
- Flutter: Filter chips row above application list (All, Identified, Reviewed, Submitted, Interviewing, Offer, Closed/Rejected)
- "Interviewing" chip maps to `status=recruiter_screen,tech_interview` (comma-separated; backend handles)
- Backend: `status` param accepts comma-separated values; Spring Data JPA `findByUserIdAndStatusIn()`
- `DashboardBloc(FilterChanged)` re-triggers `LoadApplications` with updated filter params

---

### Story 5.4: Search Applications

As an **authenticated user**,
I want to search my applications by company name or job title,
So that I can find a specific application quickly.

**Acceptance Criteria:**

**Given** I type "Acme" into the search bar on the Dashboard
**When** I stop typing (300ms debounce)
**Then** the Flutter app calls `GET /api/v1/applications?q=Acme`
**And** only applications where `company_name ILIKE '%Acme%'` OR `job_title ILIKE '%Acme%'` are shown

**Given** the search returns 0 results
**When** the list renders
**Then** an empty state shows: "No applications match 'Acme'. Try a different search."

**Given** I clear the search input
**When** the input is emptied
**Then** the full application list is restored (no `q` param in request)

**Given** I combine search with a status filter
**When** both are active
**Then** both `q` and `status` params are sent together

**Implementation notes:**
- Backend: `findByUserIdAndSearchQuery()` — JPA query with ILIKE on `job_title` and `company_name`
- Flutter: `SearchBar` widget with `DashboardBloc(SearchChanged(query))`; debounce via `RxDart` or `Timer`

---

### Story 5.5: Application Detail View

As an **authenticated user**,
I want to see the full details of any application,
So that I have all context available when preparing for interviews or follow-ups.

**Acceptance Criteria:**

**Given** I tap an application card on the Dashboard
**When** the app navigates to `/applications/:id`
**Then** `GET /api/v1/applications/{id}` is called and all fields are displayed
**And** the page shows: job title, company name, clickable job URL, status, all optional fields that have values, and the status history (from Story 4.4)

**Given** `job_url` is displayed
**When** I click/tap it
**Then** it opens in a new browser tab

**Given** `follow_up_date` is set and is today or in the past
**When** the detail page renders
**Then** the follow-up date is highlighted in amber: "Follow-up due"

**Given** the application does not belong to the authenticated user
**When** the API is called directly
**Then** the API returns HTTP 403

**Implementation notes:**
- Flutter: `ApplicationDetailPage` receives `applicationId` via `go_router` path param
- `ApplicationDetailBloc(LoadApplication(id))` → `GET /api/v1/applications/{id}` + `GET /api/v1/applications/{id}/events`
- Use `url_launcher` package for `job_url`

---

## Epic 6: Email Notifications

**Goal:** Keep users informed and prompt timely action via email at key pipeline moments — status changes, follow-up due dates, and stalled applications.

**FRs:** FR-040–046
**NFRs:** NFR-R-002, NFR-S-005

---

### Story 6.1: Stage-Change Email Notification

As an **authenticated user**,
I want to receive an email when any application status changes,
So that I have a record of every pipeline update.

**Acceptance Criteria:**

**Given** any status change is saved via `POST /api/v1/applications/{id}/status`
**When** `ApplicationService.advanceStatus()` completes successfully
**Then** `NotificationService.queueStageChangeNotification()` is called asynchronously (non-blocking)
**And** a `notification_log` row is inserted with `type = 'stage_change'`, `status = 'pending'`
**And** the email is sent via `JavaMailSender` to the user's registered email

**Given** the email is sent successfully
**When** SMTP returns success
**Then** `notification_log.status` is updated to `'sent'` and `sent_at` is set

**Given** the user has opted out of stage-change notifications
**When** `NotificationService` checks preferences
**Then** no email is sent and no `notification_log` row is created

**Email content:**
- Subject: "Application updated: {job_title} at {company_name}"
- Body: Current status, previous status, timestamp, direct link to application detail (`{APP_BASE_URL}/applications/{id}`)

**Implementation notes:**
- `@Async` annotation on notification call to avoid blocking the HTTP response
- `JavaMailSender` + `SimpleMailMessage` or `MimeMessage` for HTML template
- `APP_BASE_URL` from environment variable

---

### Story 6.2: Follow-Up Due Email Notification

As an **authenticated user**,
I want to receive an email on the day a follow-up date I set is reached,
So that I'm reminded to take action on time.

**Acceptance Criteria:**

**Given** an application has `follow_up_date` set to today's date
**When** the `NotificationScheduler` runs its daily check at 08:00 UTC
**Then** an email is sent for each such application where `status NOT IN ('closed', 'rejected')`
**And** a `notification_log` row is inserted for each sent email

**Given** `follow_up_date` is in the past (missed)
**When** the scheduler runs
**Then** overdue follow-up emails are also sent (one email per overdue application, once per scheduler run)

**Given** the user has opted out of follow-up notifications
**When** the scheduler processes their applications
**Then** no email is sent for that user's applications

**Email content:**
- Subject: "Follow-up due: {job_title} at {company_name}"
- Body: Application details, follow-up date, direct link

**Implementation notes:**
- `@Scheduled(cron = "0 0 8 * * *")` in `NotificationScheduler`
- Query: `findApplicationsWithFollowUpDateOnOrBefore(LocalDate.now())`

---

### Story 6.3: Stale Application Email Notification

As an **authenticated user**,
I want to receive an email when an application hasn't moved in 7 days,
So that stalled opportunities don't fall off my radar.

**Acceptance Criteria:**

**Given** an application has `last_status_changed_at` more than 7 days ago
**When** the `NotificationScheduler` runs its daily check
**Then** a stale-application email is sent for each qualifying application where `status NOT IN ('closed', 'rejected')`

**Given** a stale notification was already sent 7 days ago for the same application
**When** the scheduler runs again today
**Then** another stale notification is sent (repeat every 7 days per ADR-008)
**And** a new `notification_log` row is created for each re-send

**Given** the user has opted out of stale notifications
**When** the scheduler processes their applications
**Then** no stale email is sent for that user

**Email content:**
- Subject: "Still waiting? {job_title} at {company_name} hasn't moved in 7+ days"
- Body: Current status, days stalled, direct link, suggestion to update or close

**Implementation notes:**
- Query: `findActiveApplicationsNotUpdatedSince(Instant.now().minus(7, DAYS))`
- Check `notification_preferences.stale_application` JSONB field before sending
- Use `notification_log` to detect last stale send: only re-send if `last stale sent_at < NOW() - 7 days` OR no prior stale log exists

---

### Story 6.4: Email Retry with Backoff

As a **developer**,
I want failed email sends retried automatically with exponential backoff,
So that transient SMTP failures don't result in lost notifications.

**Acceptance Criteria:**

**Given** `JavaMailSender.send()` throws a `MailException` (SMTP timeout, connection refused)
**When** the retry mechanism handles the failure
**Then** the send is retried up to 3 times with delays: 5 min → 25 min → 125 min
**And** `notification_log.attempt_count` is incremented on each attempt
**And** `notification_log.last_attempted_at` is updated on each attempt

**Given** all 3 retries fail
**When** the final retry exhausts
**Then** `notification_log.status` is set to `'failed'`
**And** no further retries are attempted (prevents infinite loops)

**Given** a retry attempt succeeds
**When** SMTP accepts the message
**Then** `notification_log.status` is set to `'sent'` and `sent_at` is recorded

**Implementation notes:**
- `spring-retry` dependency; `@Retryable(maxAttempts = 3, backoff = @Backoff(delay = 300000, multiplier = 5))`
- Annotate `EmailSender.send()` method
- `@Recover` method updates `notification_log.status = 'failed'`

---

### Story 6.5: Notification Opt-Out Preferences

As an **authenticated user**,
I want to control which types of email notifications I receive,
So that I only get the emails that are useful to me.

**Acceptance Criteria:**

**Given** I navigate to Settings > Notifications
**When** the page loads
**Then** `GET /api/v1/users/me/notification-preferences` is called
**And** I see three toggles: "Stage change emails", "Follow-up due emails", "Stale application emails"
**And** all three default to ON for new accounts

**Given** I toggle "Stage change emails" to OFF and save
**When** the Flutter app calls `PUT /api/v1/users/me/notification-preferences` with `{ "stage_change": false, ... }`
**Then** the API updates `users.notification_preferences` JSONB and returns HTTP 200
**And** subsequent status changes no longer trigger stage-change emails for this user

**Given** I toggle all notifications OFF
**When** any notification trigger fires
**Then** no emails are sent and no `notification_log` rows are created for this user

**Implementation notes:**
- Backend: `GET/PUT /api/v1/users/me/notification-preferences` → read/write `users.notification_preferences` JSONB
- `NotificationPreferences` DTO: `{ stage_change: boolean, follow_up_due: boolean, stale_application: boolean }`
- `NotificationService` checks preferences before queuing any notification

---

## Epic 7: Data Integrity & Audit

**Goal:** Ensure all user data is scoped to the owner, status events are immutable, and all records carry accurate timestamps.

**FRs:** FR-051, FR-054
**NFRs:** NFR-S-006

---

### Story 7.1: User-Scoped Data Access Enforcement

As an **authenticated user**,
I want all my data to be completely isolated from other users' data,
So that no other user can see or modify my applications.

**Acceptance Criteria:**

**Given** I am authenticated as User A
**When** I call `GET /api/v1/applications/{id}` with an application ID belonging to User B
**Then** the API returns HTTP 403 (not 404, to avoid resource enumeration)
**And** no data from User B's record is returned in the response body

**Given** any repository query is executed for applications, events, or notifications
**When** the query is inspected
**Then** every query includes a `user_id = :authenticatedUserId` predicate

**Given** I call `DELETE /api/v1/applications/{id}` with User B's application ID
**Then** the API returns HTTP 403 without deleting anything

**Given** a Spring Data JPA repository method is defined
**When** the method name is reviewed
**Then** all `findById`, `existsById`, and `deleteById` variants are replaced with `findByIdAndUserId`, `existsByIdAndUserId`, `deleteByIdAndUserId`

**Implementation notes:**
- `ApplicationRepository`: use `findByIdAndUserId(UUID id, UUID userId)` — returns `Optional.empty()` if not found or wrong owner → throw `ResourceNotFoundException` (mapped to 403 by `GlobalExceptionHandler`)
- Review all repositories at story completion — add a checklist item to PR

---

### Story 7.2: Append-Only Status Event Integrity

As a **developer**,
I want status events to be permanently immutable once created,
So that the audit trail is trustworthy and can never be manipulated.

**Acceptance Criteria:**

**Given** a status event is inserted into `application_events`
**When** any code path attempts to call `UPDATE` or `DELETE` on `application_events`
**Then** no such calls exist in the application codebase (enforced by code review and repository pattern)

**Given** `ApplicationEventRepository` is defined
**When** its methods are inspected
**Then** it exposes only `save(ApplicationEvent)` and `findBy*` methods — no `delete*` or `update*` methods are defined

**Given** the `application_events` table is inspected in PostgreSQL
**When** a DBA or admin attempts `DELETE FROM application_events`
**Then** the operation succeeds only with direct DB access (application-level enforcement is sufficient for v1; DB-level trigger is a post-v1 hardening item)

**Given** a cascade-delete of an `applications` record is triggered
**When** the parent `applications` row is deleted
**Then** `application_events` rows are deleted via `ON DELETE CASCADE` (cascade only — not application-initiated delete)

**Implementation notes:**
- `ApplicationEventRepository extends JpaRepository<ApplicationEvent, UUID>` — intentionally exposes no delete methods
- Code comment in repository: `// Append-only: no delete or update operations are exposed`
- PR checklist: verify no `DELETE FROM application_events` appears anywhere in codebase

---

## Epic 8: Infrastructure & DevOps

**Goal:** Establish the Docker Compose local and production environments, Flyway schema baseline, environment variable configuration, and automated database backup — enabling reproducible deployment and fast developer onboarding.

**FRs:** (infrastructure)
**NFRs:** NFR-M-001–003, NFR-R-003, NFR-S-003–004

---

### Story 8.1: Docker Compose Local Development Environment

As a **developer**,
I want a Docker Compose dev stack that mirrors production,
So that I can develop and test locally without environment drift.

**Acceptance Criteria:**

**Given** I clone the repo and run `docker-compose -f docker-compose.dev.yml up`
**When** all services start
**Then** the following are running and healthy:
  - PostgreSQL 16 on `localhost:5432`
  - Spring Boot API on `localhost:8080` (with hot reload via `spring-boot-devtools`)
  - Flutter dev server on `localhost:8081`

**Given** I modify a Spring Boot source file
**When** the file is saved
**Then** Spring Boot devtools restarts the app and the change is live within 5 seconds

**Given** the dev environment starts for the first time
**When** Spring Boot initializes
**Then** Flyway runs all pending migrations automatically (creates `job_tracker` schema)

**Given** I need to reset the dev database
**When** I run `docker-compose -f docker-compose.dev.yml down -v`
**Then** the PostgreSQL volume is destroyed and a fresh schema is created on next `up`

**Implementation notes:**
- `docker-compose.dev.yml` at repo root
- Environment: all secrets via `.env` file (`.env.example` documents required vars)
- Volume: `postgres_dev_data` persists between `up`/`down` (destroyed only with `-v`)
- Flutter: `flutter run -d web-server --web-port 8081` or equivalent Dockerfile

---

### Story 8.2: Flyway Database Schema Baseline

As a **developer**,
I want the complete PostgreSQL schema managed by Flyway migrations,
So that schema changes are versioned, reproducible, and applied automatically on deploy.

**Acceptance Criteria:**

**Given** the application starts for the first time against an empty PostgreSQL database
**When** Flyway initializes
**Then** the following migrations are applied in order:
  - `V1__create_users.sql` — `users` table + `application_status` enum
  - `V2__create_applications.sql` — `applications` table + indexes
  - `V3__create_application_events.sql` — `application_events` table + indexes
  - `V4__create_notification_log.sql` — `notification_log` table + index
  - `V5__create_status_updated_trigger.sql` — trigger to auto-update `last_status_changed_at`

**Given** a migration has already been applied
**When** the application restarts
**Then** Flyway detects the applied checksum and skips already-applied migrations

**Given** a migration SQL file is modified after it has been applied
**When** the application starts
**Then** Flyway throws a checksum validation error and the app fails to start (intentional — prevents silent schema drift)

**Given** a new migration is added (e.g., `V6__add_applied_date_index.sql`)
**When** the app deploys to VPS
**Then** Flyway applies only the new migration

**Implementation notes:**
- Migration files in `src/main/resources/db/migration/`
- Schema matches Architecture Doc §4 exactly
- `V5` trigger: `CREATE TRIGGER update_last_status_changed BEFORE UPDATE OF status ON applications FOR EACH ROW EXECUTE FUNCTION update_last_status_changed_at()`

---

### Story 8.3: VPS Production Docker Compose Stack

As a **developer**,
I want a production Docker Compose stack deployable to a single VPS,
So that I can ship and run the application with minimal operational complexity.

**Acceptance Criteria:**

**Given** the production `docker-compose.yml` is run on a fresh Ubuntu 24.04 VPS
**When** `docker-compose up -d` completes
**Then** the following are running:
  - Nginx (ports 80, 443) serving Flutter Web SPA and proxying `/api/` to Spring Boot
  - Spring Boot API (internal port 8080)
  - PostgreSQL 16 (internal only, not exposed on host)

**Given** HTTP traffic arrives on port 80
**When** Nginx processes the request
**Then** a 301 redirect to HTTPS is issued

**Given** a user navigates to any Flutter route (e.g., `/dashboard`)
**When** Nginx serves the request
**Then** `try_files $uri $uri/ /index.html` returns `index.html` (SPA fallback)

**Given** all environment variables are set via `.env` file
**When** `docker-compose up` is run
**Then** no secrets are hardcoded in `docker-compose.yml` or any config file

**Given** Nginx responses are inspected
**When** any response is received
**Then** all 5 security headers from ADR-010 are present

**Implementation notes:**
- Flutter Web build artifacts mounted as Docker volume or copied into Nginx image
- PostgreSQL volume `postgres_data` persists across container restarts
- `.env.example` documents: `DB_USER`, `DB_PASSWORD`, `JWT_SECRET`, `SMTP_USER`, `SMTP_PASSWORD`, `APP_BASE_URL`

---

### Story 8.4: Automated Database Backup

As a **developer**,
I want the PostgreSQL database backed up daily automatically,
So that application data can be recovered in case of VPS failure or accidental deletion.

**Acceptance Criteria:**

**Given** the backup cron job is configured on the VPS
**When** it runs daily at 02:00 UTC
**Then** `pg_dump` produces a compressed `.sql.gz` backup of the `job_tracker` database
**And** the backup file is named `job_tracker_YYYY-MM-DD.sql.gz`
**And** the file is stored in `/var/backups/job-tracker/`

**Given** backups exist in `/var/backups/job-tracker/`
**When** the cleanup script runs
**Then** backup files older than 7 days are deleted

**Given** a backup file exists
**When** a restore is needed
**Then** `gunzip -c job_tracker_DATE.sql.gz | psql -U $DB_USER job_tracker` restores the database (verified in README)

**Implementation notes:**
- Cron entry: `0 2 * * * /usr/bin/pg_dump -U $DB_USER -d job_tracker | gzip > /var/backups/job-tracker/job_tracker_$(date +\%Y-\%m-\%d).sql.gz`
- Cleanup cron: `0 3 * * * find /var/backups/job-tracker -name "*.sql.gz" -mtime +7 -delete`
- Document restore procedure in `README.md`
- This is a cron-based solution on VPS host (not containerized) for simplicity

---

## Story Completion Summary

| Epic | Stories | FRs Covered | Status |
|------|---------|-------------|--------|
| E-001 Auth & Account | 1.1, 1.2, 1.3, 1.4, 1.5 | FR-001–006, FR-050, FR-052 | Ready |
| E-002 Application Capture | 2.1, 2.2, 2.3, 2.4, 2.5 | FR-010, FR-014–018, FR-053 | Ready |
| E-003 URL Intake & Parsing | 3.1, 3.2, 3.3, 3.4 | FR-011–013 | Ready |
| E-004 Pipeline Management | 4.1, 4.2, 4.3, 4.4, 4.5 | FR-020–026 | Ready |
| E-005 Dashboard & Visibility | 5.1, 5.2, 5.3, 5.4, 5.5 | FR-030–036 | Ready |
| E-006 Email Notifications | 6.1, 6.2, 6.3, 6.4, 6.5 | FR-040–046 | Ready |
| E-007 Data Integrity & Audit | 7.1, 7.2 | FR-051, FR-054, NFR-S-006 | Ready |
| E-008 Infrastructure & DevOps | 8.1, 8.2, 8.3, 8.4 | NFR-M-001–003, NFR-R-003, NFR-S-003–004 | Ready |
| **Total** | **35 stories** | **All 54 FRs covered** | **Ready for Dev** |

---

## Recommended Sprint Sequence

| Sprint | Epics | Stories | Goal |
|--------|-------|---------|------|
| Sprint 0 | E-008 | 8.1, 8.2, 8.3, 8.4 | Working dev + prod infra; DB schema live |
| Sprint 1 | E-001 | 1.1, 1.2, 1.3, 1.4, 1.5 | Auth working end-to-end; secure sessions |
| Sprint 2 | E-002, E-003 | 2.1–2.5, 3.1–3.4 | Applications created manually + via URL paste |
| Sprint 3 | E-004, E-007 | 4.1–4.5, 7.1, 7.2 | Full pipeline lifecycle; audit trail; data isolation |
| Sprint 4 | E-005 | 5.1–5.5 | Dashboard fully functional; filter + search |
| Sprint 5 | E-006 | 6.1–6.5 | All email notifications live; retry working |

---

_Generated by Bob (BMad SM Agent) · job-tracker-planning · 2026-03-27_
_Input: PRD v1.0, Architecture Doc v1.0_

