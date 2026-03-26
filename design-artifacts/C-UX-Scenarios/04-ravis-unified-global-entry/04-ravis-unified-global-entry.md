---
design_intent: S
design_status: not-started
---

# 04: Ravi's Unified Global Entry

**Project:** job-tracker-planning
**Created:** 2026-03-26
**Method:** Whiteport Design Studio (WDS)

---

## Transaction (Q1)

**What this scenario covers:**
Ravi wants one application that does it all, so he can manage his global job-search process in one place.

---

## Business Goal (Q2)

**Goal:** Increase job search throughput for users
**Objective:** Increase application submissions by reducing fragmentation across job sources

---

## User & Situation (Q3)

**Persona:** Ravi (Tertiary)
**Situation:** Ravi is switching between multiple job sources and wants a unified workflow.

---

## Driving Forces (Q4)

**Trigger:** Ravi has found a promising role on another site and wants to move it into one central workflow immediately.

**Hope:** Ravi can move easily between websites for positions and enter data without friction.

**Worry:** The application feels clunky.

> CONSTRAINT: One sentence per component. Phrases, not paragraphs.

---

## Device & Starting Point (Q5 + Q6)

**Device:** Desktop
**Entry:** Ravi pastes a job URL from another site into the add flow.

---

## Best Outcome (Q7)

**User Success:**
The URL is parsed and the data is mapped to the required fields.

**Business Success:**
More externally discovered roles are captured into the pipeline, supporting higher submission volume across job sources.

---

## Shortest Path (Q8)

1. **Job URL Paste** — Ravi pastes a job URL from an external source into the add flow.
2. **Parsed Data Review** — The system shows mapped fields and extracted details for quick review.
3. **Complete Required Fields** — Ravi fills any required missing fields with minimal effort.
4. **Save Application** — The opportunity is saved into the unified workflow and ready for progression. ✓

---

## Trigger Map Connections

**Persona:** Ravi (Tertiary)

**Driving Forces Addressed:**
- ✅ **Want:** Manage global opportunities in one consistent system
- ❌ **Fear:** Fragmented source tracking and duplicate effort

**Business Goal:** Increase throughput by making it easier to convert cross-site discoveries into tracked applications.

---

## Scenario Steps

Steps are outlined one at a time after scenario creation. The first step is processed automatically.

| Step | Folder | Purpose | Exit Action |
|------|--------|---------|-------------|
| 4.1 | `4.1-job-url-paste/` | Start unified capture from an external job URL | Triggers parse and mapping workflow |
| 4.2 | `4.2-parsed-data-review/` | Review extracted data and verify key fields | Continues to required-field completion |
| 4.3 | `4.3-complete-required-fields/` | Fill any missing required fields quickly | Submits complete record |
| 4.4 | `4.4-save-application/` | Save the application into the central workflow | Returns to ready-to-progress state ✓ |

**First step** (`4.1`) includes full entry context (Q3 + Q4 + Q5 + Q6).
**On-step interactions** (that do not leave the step) are documented as storyboard items within each page spec.

