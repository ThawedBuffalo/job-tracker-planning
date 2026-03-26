# Design Log

**Project:** job-tracker-planning
**Started:** 2026-03-26
**Method:** Whiteport Design Studio (WDS)

---

## Backlog

> Business-value items. Add links to detail files if needed.

- [x] Complete product brief — Phase 1
- [x] Define trigger map — Phase 2
- [x] Create user scenarios — Phase 3
- [x] Detail page specifications — Phase 4
- [x] Prepare design delivery package — Phase 4

---

## Current

| Task | Started | Agent |
|------|---------|-------|

**Rules:** Mark what you start. Complete it when done (move to Progress/Log). One task at a time per agent.

---

## Design Loop Status

> Per-page design progress. Updated by agents at every design transition.

| Scenario | Step | Page | Status | Updated |
|----------|------|------|--------|---------|
| 01-priyas-core-pipeline-momentum | 1.1 | Home | building | 2026-03-26 |
| 01-priyas-core-pipeline-momentum | 1.2 | Dashboard | building | 2026-03-26 |
| 01-priyas-core-pipeline-momentum | 1.3 | Add Application | building | 2026-03-26 |
| 01-priyas-core-pipeline-momentum | 1.4 | Confirm Saved | building | 2026-03-26 |
| 02-claras-follow-up-confidence-loop | 2.1 | Email Link Entry | specified | 2026-03-26 |
| 02-claras-follow-up-confidence-loop | 2.2 | Application Detail | specified | 2026-03-26 |
| 02-claras-follow-up-confidence-loop | 2.3 | Next-Step Guidance | specified | 2026-03-26 |
| 02-claras-follow-up-confidence-loop | 2.4 | Confirm Next Action | specified | 2026-03-26 |
| 03-ethans-required-field-fast-entry | 3.1 | Job URL Paste | specified | 2026-03-26 |
| 03-ethans-required-field-fast-entry | 3.2 | Required Fields Prompt | specified | 2026-03-26 |
| 03-ethans-required-field-fast-entry | 3.3 | Complete Missing Fields | specified | 2026-03-26 |
| 03-ethans-required-field-fast-entry | 3.4 | Save Application | specified | 2026-03-26 |
| 04-ravis-unified-global-entry | 4.1 | Job URL Paste | specified | 2026-03-26 |
| 04-ravis-unified-global-entry | 4.2 | Parsed Data Review | specified | 2026-03-26 |
| 04-ravis-unified-global-entry | 4.3 | Complete Required Fields | specified | 2026-03-26 |
| 04-ravis-unified-global-entry | 4.4 | Save Application | specified | 2026-03-26 |

**Status values:** `discussed` -> `wireframed` -> `specified` -> `explored` -> `building` -> `built` -> `approved` | `removed`

---

## Progress

### 2026-03-26 — Phase 1: Product Brief Complete

**Agent:** Saga (Product Brief)
**Brief Level:** complete

**Artifacts Created:**
- `design-artifacts/A-Product-Brief/project-brief.md` — Product brief
- `design-artifacts/A-Product-Brief/platform-requirements.md` — Platform requirements

**Summary:** Established the core product direction for `job-tracker-planning` as a responsive web/mobile-web workflow tool for individual technical job seekers. Locked the canonical application status model, email-notification scope, MVP boundaries, and single-VPS Flutter Web + Spring Boot + PostgreSQL delivery approach.

**Next:** Phase 2 — Trigger Mapping

### 2026-03-26 — Phase 2: Trigger Mapping Complete

**Agent:** Saga (Trigger Mapping)
**Personas:** 4 (Priya, Ethan, Clara, Ravi)
**Business Goals:** 3

**Artifacts Created:**
- `design-artifacts/B-Trigger-Map/trigger-map.md` — Visual overview and strategic map
- `design-artifacts/B-Trigger-Map/feature-impact-analysis.md` — Prioritized feature impact analysis
- `design-artifacts/B-Trigger-Map/personas/02-priya-the-pipeline-optimizer.md` — Primary persona
- `design-artifacts/B-Trigger-Map/personas/03-ethan-the-efficient-explorer.md` — Secondary persona
- `design-artifacts/B-Trigger-Map/personas/03-clara-the-career-changer.md` — Secondary persona
- `design-artifacts/B-Trigger-Map/personas/04-ravi-the-remote-reacher.md` — Tertiary persona

**Summary:** Prioritized Priya as the primary design target, then mapped supporting behaviors for Ethan, Clara, and Ravi. Established four highest-impact workflow themes around fast capture, required-field completeness, guided next steps, and unified multi-source intake.

**Next:** Phase 3 — UX Scenarios

### 2026-03-26 — Phase 3: UX Scenarios Complete

**Agent:** Saga (Scenario Outline)
**Scenarios:** 4 scenarios covering 16 pages
**Quality:** Good

**Artifacts Created:**
- `design-artifacts/C-UX-Scenarios/00-ux-scenarios.md` — Scenario index
- `design-artifacts/C-UX-Scenarios/01-priyas-core-pipeline-momentum/01-priyas-core-pipeline-momentum.md` — Priya's Fast Job Entry
- `design-artifacts/C-UX-Scenarios/01-priyas-core-pipeline-momentum/1.1-home/1.1-home.md` — Scenario 01 first-step page outline
- `design-artifacts/C-UX-Scenarios/02-claras-follow-up-confidence-loop/02-claras-follow-up-confidence-loop.md` — Clara's Follow-Up Confidence Loop
- `design-artifacts/C-UX-Scenarios/02-claras-follow-up-confidence-loop/2.1-email-link-entry/2.1-email-link-entry.md` — Scenario 02 first-step page outline
- `design-artifacts/C-UX-Scenarios/03-ethans-required-field-fast-entry/03-ethans-required-field-fast-entry.md` — Ethan's Required-Field Fast Entry
- `design-artifacts/C-UX-Scenarios/03-ethans-required-field-fast-entry/3.1-job-url-paste/3.1-job-url-paste.md` — Scenario 03 first-step page outline
- `design-artifacts/C-UX-Scenarios/04-ravis-unified-global-entry/04-ravis-unified-global-entry.md` — Ravi's Unified Global Entry
- `design-artifacts/C-UX-Scenarios/04-ravis-unified-global-entry/4.1-job-url-paste/4.1-job-url-paste.md` — Scenario 04 first-step page outline

**Summary:** Created four persona-led storyboard scenarios covering fast entry, guided follow-up, required-field enforcement, and unified multi-source intake. Kept scope to a small dynamic app, used storyboard format throughout, and assigned all 16 scenario pages without duplication to preserve clean Phase 4 handoff.

**Next:** Phase 4 — UX Design

---

## Key Decisions

| Date | Decision | Context | Participants |
|------|----------|---------|--------------|
| 2026-03-26 | v1 scope locked to responsive web + mobile web, email notifications, manual entry + URL parsing, and no auto-apply | Phase 1: Product Brief | Saga + JC |
| 2026-03-26 | Core stack chosen as Flutter Web + Spring Boot + PostgreSQL on a single VPS with Docker Compose | Phase 1: Platform Requirements | Saga + JC |
| 2026-03-26 | Priya selected as primary design target; trigger map expanded to four personas and feature priorities centered on guided workflow, required fields, and multi-source intake | Phase 2: Trigger Mapping | Saga + JC |
| 2026-03-26 | UX scenario plan fixed at 4 storyboard scenarios with persona-based naming and full 16-page coverage | Phase 3: Scenarios | Saga + JC |
| 2026-03-26 | Design intent set to Suggest (S) for all four scenarios to guide Phase 4 page design step-by-step | Phase 4 Prep: Handover | Saga + JC |

---

## Log

### 2026-03-26 — Design Delivery: DD-001 Application Capture + Follow-Up

**Agent:** Freya (WDS Phase 4 — Handover)
**Delivery:** DD-001
**Scenarios packaged:** 4 (01 Priya, 02 Clara, 03 Ethan, 04 Ravi)
**Pages:** 16 specified

**Artifacts Created:**
- `design-artifacts/E-PRD/Design-Deliveries/DD-001-application-capture-and-follow-up.yaml`
- `design-artifacts/E-PRD/Design-Deliveries/TS-001-application-capture-and-follow-up.yaml`

**Summary:** Packaged all 16 Phase 4 page specs into a single Design Delivery covering authentication, application CRUD, URL parsing, email deep-link routing, required-field validation, and guided next-step recommendations. Test scenario covers 4 happy paths, 3 error states, 2 edge cases, 2 accessibility checks, and 2 usability tests.

**Next:** Phase 5 — Agentic Development (bmad-wds-agentic-development)

---

### 2026-03-26 — Design log initialized from completed Phase 1-3 artifacts
- Reason: no prior `_bmad-output/_progress/00-design-log.md` existed in the workspace
- Action: reconstructed baseline project progress from approved Product Brief, Trigger Map, and UX Scenario outputs

---

## About This Folder

- **This file** — Single source of truth for project progress
- **agent-experiences/** — Compressed insights from design discussions (dated files)
- **_bmad-output/** — Working output folder for workflow tracking

