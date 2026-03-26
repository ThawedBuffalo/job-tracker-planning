---
design_intent: S
design_status: not-started
---

# 01: Priya's Fast Job Entry

**Project:** job-tracker-planning
**Created:** 2026-03-26
**Method:** Whiteport Design Studio (WDS)

---

## Transaction (Q1)

**What this scenario covers:**
Easily enter job description information so Priya can quickly create a usable application record.

---

## Business Goal (Q2)

**Goal:** Make application management operationally simple
**Objective:** Improve speed and consistency of capture/updates

---

## User & Situation (Q3)

**Persona:** Priya (Primary)
**Situation:** Priya is in a focused evening job-search session on desktop and wants to log a role quickly without breaking flow.

---

## Driving Forces (Q4)

**Trigger:** Priya has just found a promising role and wants to log it before momentum fades.

**Hope:** Data entry is optimized so she can add job details quickly.

**Worry:** It turns into a lot of manual typing.

> CONSTRAINT: One sentence per component. Phrases, not paragraphs.

---

## Device & Starting Point (Q5 + Q6)

**Device:** Desktop
**Entry:** Priya opens the bookmarked app URL and signs in.

---

## Best Outcome (Q7)

**User Success:**
Priya accesses the system and sees the dashboard.

**Business Success:**
Priya reaches the dashboard and starts the job-entry workflow in the same session, increasing active capture usage.

---

## Shortest Path (Q8)

1. **Home** — Priya arrives at the sign-in home and authenticates.
2. **Dashboard** — She selects the add-application action.
3. **Add Application** — She enters job description information and submits.
4. **Confirm Saved** — She sees successful save confirmation on the dashboard. ✓

---

## Trigger Map Connections

**Persona:** Priya (Primary)

**Driving Forces Addressed:**
- ✅ **Want:** Keep momentum with low-friction updates
- ❌ **Fear:** Losing track across tools

**Business Goal:** Operational simplicity through faster, lower-friction data capture.

---

## Scenario Steps

Steps are outlined one at a time after scenario creation. The first step is processed automatically.

| Step | Folder | Purpose | Exit Action |
|------|--------|---------|-------------|
| 1.1 | `1.1-home/` | Authenticate and enter workspace | Submits credentials and lands on dashboard |
| 1.2 | `1.2-dashboard/` | Start application entry from the main workspace | Selects add-application action |
| 1.3 | `1.3-add-application/` | Enter job description information and submit | Saves the application |
| 1.4 | `1.4-confirm-saved/` | Verify success confirmation and readiness for next action | Returns to dashboard state ✓ |

**First step** (`1.1`) includes full entry context (Q3 + Q4 + Q5 + Q6).
**On-step interactions** (that do not leave the step) are documented as storyboard items within each page spec.

