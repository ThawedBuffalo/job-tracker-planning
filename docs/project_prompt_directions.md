Great research gathered. Here's a comprehensive guide for applying the BMad Method to your multi-repo Flutter + Spring Boot microservices job tracker system.

---

## Applying the BMad Method to Your Job Tracker System

### What BMad Is (and Why It Fits This Project)

The BMad Method (Build More Architect Dreams) is an AI-driven development framework that helps you build software through the whole process from ideation and planning all the way through agentic implementation. It's particularly well-suited for your situation because BMAD introduces focused AI personas — Product Manager, Architect, Developer, Scrum Master, UX Designer, and more — each defined as an "Agent-as-Code" Markdown file describing expertise, responsibilities, constraints, and expected outputs.

The key advantage for a multi-repo polyglot system like yours: the multi-agent approach to planning eliminates the common problem of context loss. Each planning agent builds upon the work of its predecessors, creating a cumulative understanding that rivals the collective intelligence of an experienced development team.

---

### Phase 1 — Planning (Before You Touch a Repo)

Run these BMad agents **in a single planning repo** (e.g., `job-tracker-planning`) before creating any source repos.

**Step 1: Install BMad**

```bash
npx bmad-method install
```

Follow the prompts. Then open your AI IDE (Claude Code, Cursor, etc.) in your project folder.

**Step 2: Run the Analyst Agent**

Activate `*analyst` and describe your system. The agent will produce a Project Brief covering:
- Core job tracking features (create/read/update jobs, status pipelines, notes, reminders)
- User personas (job seeker, recruiter-view if applicable)
- Integration constraints (local PostgreSQL, no cloud dependency)

**Step 3: Run the PM Agent → PRD**

Activate `*pm`. It consumes the Analyst output and produces a full **Product Requirements Document** with epics and features broken down by layer:
- Flutter mobile/web features
- API contract definitions
- Microservice responsibilities

**Step 4: Run the Architect Agent → Architecture Doc**

This is the most critical step for your multi-repo setup. Activate `*architect` and feed it your PRD. The Architect agent will produce:

- **System Architecture Doc** — defining your service boundaries
- **API contracts** (OpenAPI specs per service)
- **Data model** (PostgreSQL schema)
- **Repo structure decision** (monorepo vs. polyrepo)

For your system, the recommended repo split coming out of this phase should look like:

```
job-tracker-planning/       ← BMad artifacts, PRD, arch docs, stories
job-tracker-flutter/        ← Flutter mobile + web frontend
job-tracker-api-gateway/    ← Spring Boot gateway/BFF service
job-tracker-job-service/    ← Core job CRUD microservice
job-tracker-user-service/   ← Auth + user profile microservice
job-tracker-notification-service/ ← Reminders, alerts
job-tracker-shared-libs/    ← Shared Java DTOs, exceptions, OpenAPI specs
job-tracker-infra/          ← Docker Compose, DB migrations (Flyway/Liquibase)
```

---

### Phase 2 — Story Generation (Scrum Master Agent)

The Scrum Master agent transforms planning documents into hyper-detailed development stories that embed full context, implementation details, and architectural guidance — eliminating "planning inconsistency" and "context loss."

Activate `*sm` in your planning repo. It reads the Architecture Doc and PRD and generates **story files** — one per feature, per repo. For example:

- `story-001-job-service-crud-endpoints.md` → goes into `job-tracker-job-service/.bmad/stories/`
- `story-002-flutter-job-list-screen.md` → goes into `job-tracker-flutter/.bmad/stories/`
- `story-003-gateway-auth-routing.md` → goes into `job-tracker-api-gateway/.bmad/stories/`

Each story file contains the **full architectural context**, acceptance criteria, and implementation hints so the Dev agent in that repo has everything it needs without re-reading cross-repo docs.

---

### Phase 3 — Implementation (Dev Agent, Per Repo)

Once your PRD, Architecture, UX plans, and briefs are complete, the Dev and QA agents coordinate via story files. Open each source repo in Claude Code (or Cursor), point it at the story file, and activate `*dev`.

**Per-repo BMad setup:**

Each source repo gets a minimal `.bmad/` folder copied from the planning repo:

```
job-tracker-job-service/
  .bmad/
    context/
      architecture.md      ← excerpt relevant to this service
      api-contracts.yaml   ← this service's OpenAPI spec
    stories/
      story-001-crud.md
      story-004-search.md
  src/
    main/java/...
```

The Dev agent reads the story + context and generates Spring Boot code without needing to re-derive design decisions.

---

### Phase 4 — QA Agent

Every artifact — from the PRD to the architecture and granular stories — is treated as a versioned asset. This ensures traceability and accountability: every change, whether from a human or an AI agent, is thoroughly tracked.

Activate `*qa` in each repo after implementation. It:
- Reviews generated code against acceptance criteria in the story file
- Generates test cases (JUnit for Spring Boot, widget tests for Flutter)
- Flags architectural drift (e.g., if a service is calling another service's DB directly)

---

### Multi-Repo Coordination Strategy

The biggest challenge with polyrepo + BMad is keeping the **planning repo as the single source of truth**. Here's how to manage it:

| Concern | Solution |
|---|---|
| API contract drift | Publish OpenAPI specs from `job-tracker-shared-libs` via CI; all services and Flutter app consume them |
| Story cross-references | Stories reference each other by ID; planning repo `STORY-MAP.md` tracks dependencies |
| DB schema changes | All migrations live in `job-tracker-infra`; services never own their schema |
| Context for Dev agent | Each repo's `.bmad/context/` folder is a curated excerpt from the planning repo, updated manually or via a sync script |

---

### Recommended Tooling Alongside BMadHere's the full system architecture diagram to anchor everything:---

### Practical Setup Checklist

Here's the order of operations when you sit down to start:

**Day 1 — Planning repo only**
1. `mkdir job-tracker-planning && cd job-tracker-planning && git init`
2. `npx bmad-method install`
3. Open in Claude Code: activate `*analyst` → brief → `*pm` → PRD → `*architect` → Architecture Doc
4. Git commit all artifacts: PRD, arch doc, OpenAPI specs, data model

**Day 2 — Source repos + story generation**
5. Create all 7 source repos (Flutter, gateway, each service, shared-libs, infra)
6. Back in planning repo: activate `*sm`, generate stories per epic, tag them by target repo
7. Copy each story file + a curated `.bmad/context/` excerpt into the relevant repo

**Day 3+ — Implementation loop**
8. Open `job-tracker-job-service` in Claude Code
9. Activate `*dev`, point it at `story-001-crud.md`
10. Let it scaffold the Spring Boot service; commit per story
11. Activate `*qa` to generate JUnit tests against acceptance criteria
12. Repeat per repo/story

### Tips Specific to Your Stack

For **Flutter**, tell the Architect agent to define the API contract as REST+JSON (not gRPC) since Dart's gRPC support is functional but adds friction. The Flutter Dev agent context should include the OpenAPI spec from `shared-libs` so it can generate typed Dart models automatically.

For **Spring Boot microservices**, each service's `.bmad/context/` should include: its own OpenAPI slice, the relevant PostgreSQL schema tables, and a one-page service boundary description. This prevents the Dev agent from reaching across service boundaries.

For **PostgreSQL**, put all Flyway/Liquibase migrations in `job-tracker-infra` and never let a service own its own migrations. The Architect doc should make this an explicit constraint so the QA agent flags violations.

For **multi-repo coordination**, a lightweight `STORY-MAP.md` in the planning repo tracking story IDs, target repos, and statuses is far more effective than trying to use project management tooling — the Dev agent can read it as context.

---

BMad is 100% free and open source, intentionally minimal so teams can adopt it without killing velocity. The official docs are at [docs.bmad-method.org](https://docs.bmad-method.org) and Claude Code is their recommended tool, which makes this setup particularly smooth.

Since you'll be doing a lot of agentic coding across multiple repos, Claude Code is worth installing if you haven't already:Want me to go deeper on any specific layer — for example, the Spring Boot service structure, the Flutter BFF API client pattern, the PostgreSQL schema for job tracking, or how to write effective BMad story files for this domain?