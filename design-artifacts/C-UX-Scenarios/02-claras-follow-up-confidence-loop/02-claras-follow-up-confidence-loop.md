---
design_intent: S
design_status: not-started
---

# 02: Clara's Follow-Up Confidence Loop

**Project:** job-tracker-planning
**Created:** 2026-03-26
**Method:** Whiteport Design Studio (WDS)

---

## Transaction (Q1)

**What this scenario covers:**
Clara is guided through each phase of the application process so she knows exactly what to do next.

---

## Business Goal (Q2)

**Goal:** Improve user consistency and follow-through
**Objective:** Increase timely follow-up completion and reduce missed stages

---

## User & Situation (Q3)

**Persona:** Clara (Secondary)
**Situation:** Clara just received a status update and needs clear guidance on the immediate next step.

---

## Driving Forces (Q4)

**Trigger:** Clara has just received a status update and needs to respond without missing the next stage.

**Hope:** The system provides clear next steps.

**Worry:** The workflow is ambiguous and important stages get missed.

> CONSTRAINT: One sentence per component. Phrases, not paragraphs.

---

## Device & Starting Point (Q5 + Q6)

**Device:** Desktop
**Entry:** Clara opens the app from a status-change email link.

---

## Best Outcome (Q7)

**User Success:**
Current status is clearly displayed, with a breadcrumb path of upcoming stages.

**Business Success:**
Clara completes the follow-up workflow from the alert path, improving timely action completion and reducing stalled applications.

---

## Shortest Path (Q8)

1. **Email Link Entry** — Clara opens the status-change email and enters the app directly to the relevant application.
2. **Application Detail** — She sees the current status and a breadcrumb view of upcoming stages.
3. **Next-Step Guidance** — The app presents the recommended next action and why it is needed now.
4. **Confirm Next Action** — Clara confirms or schedules the next action and returns with confidence. ✓

---

## Trigger Map Connections

**Persona:** Clara (Secondary)

**Driving Forces Addressed:**
- ✅ **Want:** Step-by-step clarity for unfamiliar stages
- ❌ **Fear:** Uncertainty after each status change

**Business Goal:** Improve consistency and follow-through by reducing missed stage actions.

---

## Scenario Steps

Steps are outlined one at a time after scenario creation. The first step is processed automatically.

| Step | Folder | Purpose | Exit Action |
|------|--------|---------|-------------|
| 2.1 | `2.1-email-link-entry/` | Enter directly from status-change notification to the relevant application context | Opens application detail view |
| 2.2 | `2.2-application-detail/` | Understand current stage and upcoming stage sequence | Opens next-step guidance |
| 2.3 | `2.3-next-step-guidance/` | Understand the recommended action and timing | Selects confirm/schedule action |
| 2.4 | `2.4-confirm-next-action/` | Commit the next action and lock follow-through | Saves action and returns with confidence ✓ |

**First step** (`2.1`) includes full entry context (Q3 + Q4 + Q5 + Q6).
**On-step interactions** (that do not leave the step) are documented as storyboard items within each page spec.

