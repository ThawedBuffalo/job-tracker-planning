# Project Brief: job-tracker-planning

> Complete Strategic Foundation

**Created:** 2026-03-26
**Author:** JC
**Brief Type:** Complete

---

## Vision

Build a focused product for individual technical job seekers that replaces scattered tracking methods (spreadsheets, notes, and generic tools) with one guided workflow that helps users move more opportunities forward each week.

The product succeeds when users can consistently manage application progress across multiple sources with less manual overhead, clearer next actions, and timely reminders.

---

## Positioning Statement

For individual technical job seekers in the global market who need a simple, reliable way to manage applications across many sources, `job-tracker` is a centralized workflow tool that tracks every application stage and guides users on what to do next. Unlike generic tools such as Notion, Excel, and handwritten notes, it combines structured pipeline progression with proactive email notifications.

**Breakdown:**

- **Target Customer:** Individual technical job seekers (software engineers, QA, data, DevOps, IT)
- **Need/Opportunity:** Replace fragmented manual tracking with clear, actionable end-to-end workflow management
- **Category:** Job application workflow and tracking tool
- **Key Benefit:** One place to track every stage across multiple sources with guided next steps
- **Differentiator:** Guided workflow + email notifications for stage changes and stalled applications

---

## Business Model

**Type:** Free (no monetization in v1)

Rationale:
- Prioritize adoption, retention, and measurable workflow impact first
- Validate core value before introducing pricing experiments

---

## Ideal Customer Profile (ICP)

Individual technical professionals actively searching for jobs worldwide and applying across multiple channels (company sites, job boards, referrals, recruiters) who currently use improvised systems and need better process control.

Key characteristics:
- Manages multiple active applications in parallel
- Needs visibility into stage-by-stage progress
- Values practical productivity improvements over complex tooling
- Wants follow-up prompts and status clarity without heavy setup

### Secondary Users

- Career switchers entering technical roles
- Early-career candidates applying at high volume
- Contractors/freelancers tracking role pipelines

---

## Success Criteria

### Primary Outcome (6 Months)

- Increase **applications per week by 25%** relative to user baseline

### Supporting Metrics

- Conversion rate: `identified -> submitted`
- Conversion rate: `submitted -> recruiter_screen`
- Median time: `identified -> submitted`
- Share of applications with follow-up completed on time

### Measurement Notes

- Establish baseline applications/week at onboarding
- Track rolling weekly average to smooth out outliers
- Evaluate sustained uplift rather than one-week spikes

---

## Competitive Landscape

Current alternatives:
- Notion
- Excel
- Handwritten notes

Observed gaps in alternatives:
- No dedicated application lifecycle model
- Inconsistent stage naming and progression
- Weak follow-up reminders and low actionability
- Limited pipeline analytics for decision-making

### Our Unfair Advantage

A purpose-built workflow that does not just store status, but actively guides stage progression and prompts action via email when movement happens or stalls.

---

## Constraints

- **Timeline:** 1 month (v1)
- **Team:** Solo engineer
- **Budget:** No budget constraints

Scope implications:
- Favor high-leverage features over broad integrations
- Keep architecture simple and delivery-focused
- Defer non-critical automation until post-v1

---

## Platform & Device Strategy

**Primary Platform:** Responsive web + mobile web

**Supported Devices:**
- Desktop browsers
- Mobile browsers

**Device Priority:**
1. Desktop web for active tracking and planning
2. Mobile web for quick updates and follow-up checks

**Interaction Models:**
- Structured pipeline workflow
- Form-based application entry
- URL paste intake with parsed metadata
- Timeline/status history view

**Technical Requirements:**
- **Offline Functionality:** Not required for v1
- **Native Features:** None required for v1

**Platform Rationale:**
A single responsive web implementation delivers cross-device coverage quickly for a solo engineer, with lower complexity than native apps.

**Future Platform Plans:**
- Evaluate native mobile app only after workflow/value validation
- Evaluate deeper source integrations after manual+URL intake proves effective

**Design Implications:**
- Clear, compact stage controls for small screens
- Fast capture flow for adding opportunities
- Strong visual cues for next action and stalled states

**Development Implications:**
- Shared UI patterns across desktop/mobile web
- Notification/event model designed for future channel expansion
- Data model optimized for stage transitions and history

---

## Core Workflow Model

### Canonical Application Statuses

1. `identified`
2. `reviewed`
3. `customized_resume`
4. `submitted`
5. `first_contact`
6. `recruiter_screen`
7. `tech_interview`
8. `offer`
9. `closed`

Terminal alternative outcome:
- `rejected`

### Transition Principles

- Forward progression by default
- Manual override allowed for corrections
- `rejected` and `closed` are terminal states
- Every status update records timestamp and audit history

---

## MVP Scope (v1)

### In Scope

- Manual application entry
- Job URL paste with parsing support
- Guided stage workflow and next-step prompts
- Email notifications for:
  - Stage changes
  - Follow-up due reminders
  - Stalled applications (7 days without status change)
- Pipeline visibility and status history

### Out of Scope

- Auto-apply functionality
- Native iOS/Android apps
- Full direct integrations with external job platforms
- Advanced automation beyond core reminders

---

## Tone of Voice

**For UI Microcopy & System Messages**

### Tone Attributes

1. **Clear**: Use plain, action-oriented language with no ambiguity
2. **Encouraging**: Reduce stress and support momentum
3. **Professional**: Stay concise, respectful, and practical

### Examples

**Error Messages:**
- ✅ "Could not save this update. Please try again in a moment."
- ❌ "Something went wrong."

**Button Text:**
- ✅ "Move to Recruiter Screen"
- ❌ "Update"

**Empty States:**
- ✅ "No active applications yet. Add one to start your pipeline."
- ❌ "Nothing here."

**Success Messages:**
- ✅ "Application moved to Recruiter Screen. Next: prepare role-specific talking points."
- ❌ "Done."

### Guidelines

**Do:**
- Give explicit next-action cues
- Keep phrasing short and unambiguous
- Confirm progress in motivating language

**Don’t:**
- Use vague system messages
- Use overly casual or slang-heavy copy
- Add noise that hides the next action

---

## Additional Context

This brief assumes a brownfield-friendly product planning process where implementation details will be elaborated in downstream artifacts (Trigger Map, design specs, architecture, epics/stories).

Known strategic emphasis:
- Rapid v1 delivery with meaningful user impact
- Workflow clarity over feature breadth
- Practical reminders over complex automation

---

## Business Context

- **Primary Goal:** Improve job-seeking throughput and execution quality for individual technical candidates
- **Solution:** Centralized application workflow with guided progression and email-based action reminders
- **Target Users:** Individual technical job seekers in the global market

---

## Next Steps

- [ ] **Phase 2: Trigger Mapping** - Connect business goals to user psychology and behavioral drivers
- [ ] **Phase 3: UX Scenarios and Specifications** - Define flows, edge cases, and interaction details
- [ ] **Phase 4: Design Delivery** - Package implementation-ready artifacts
- [ ] **Phase 5: Build Planning** - Translate into architecture, epics, and stories

---

_Generated for Web Design Studio planning flow_
