---
design_intent: S
design_status: not-started
---

# 03: Ethan's Required-Field Fast Entry

**Project:** job-tracker-planning
**Created:** 2026-03-26
**Method:** Whiteport Design Studio (WDS)

---

## Transaction (Q1)

**What this scenario covers:**
The system prompts for required fields and does not allow save until required information is completed.

---

## Business Goal (Q2)

**Goal:** Improve user consistency and follow-through
**Objective:** Reduce incomplete records and prevent opportunities from being forgotten

---

## User & Situation (Q3)

**Persona:** Ethan (Secondary)
**Situation:** Ethan is adding several jobs quickly and tries to skip details to save time.

---

## Driving Forces (Q4)

**Trigger:** Ethan is batch-adding roles and hits validation before he can save an incomplete record.

**Hope:** Each job position includes all required data.

**Worry:** The record will be incomplete and later forgotten.

> CONSTRAINT: One sentence per component. Phrases, not paragraphs.

---

## Device & Starting Point (Q5 + Q6)

**Device:** Desktop
**Entry:** Ethan pastes a job URL into the add form, then encounters missing required fields before save.

---

## Best Outcome (Q7)

**User Success:**
Ethan can enter data easily and does not miss required fields.

**Business Success:**
More applications are saved with complete minimum data, increasing repeat usage and reducing abandoned records.

---

## Shortest Path (Q8)

1. **Job URL Paste** — Ethan pastes the job URL and starts the add-application flow.
2. **Required Fields Prompt** — The system identifies missing mandatory information before save.
3. **Complete Missing Fields** — Ethan fills only the required missing data quickly.
4. **Save Application** — The record saves successfully with complete minimum data. ✓

---

## Trigger Map Connections

**Persona:** Ethan (Secondary)

**Driving Forces Addressed:**
- ✅ **Want:** Reduce overhead per entry and update
- ❌ **Fear:** Entry friction causing abandoned tracking

**Business Goal:** Improve consistency and follow-through by preventing incomplete records from being dropped.

---

## Scenario Steps

Steps are outlined one at a time after scenario creation. The first step is processed automatically.

| Step | Folder | Purpose | Exit Action |
|------|--------|---------|-------------|
| 3.1 | `3.1-job-url-paste/` | Start quick intake from a pasted job URL | Triggers field extraction and validation |
| 3.2 | `3.2-required-fields-prompt/` | Show exactly which required fields still need completion | Continues to required-field entry |
| 3.3 | `3.3-complete-missing-fields/` | Fill only the missing minimum data needed to save | Submits completed record |
| 3.4 | `3.4-save-application/` | Confirm successful save of a complete-enough application record | Returns to workflow state ✓ |

**First step** (`3.1`) includes full entry context (Q3 + Q4 + Q5 + Q6).
**On-step interactions** (that do not leave the step) are documented as storyboard items within each page spec.

