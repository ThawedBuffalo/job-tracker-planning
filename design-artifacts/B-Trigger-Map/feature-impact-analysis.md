# Feature Impact Analysis: job-tracker-planning

## Scoring

**Primary Persona (Priya):** High = 5 pts | Medium = 3 pts | Low = 1 pt  
**Other Personas:** High = 3 pts | Medium = 1 pt | Low = 0 pts

**Max Possible Score:** 14 (with 4 personas)  
**Must Have Threshold:** 9+ or Primary High (5)

---

## Prioritized Features

| Rank | Feature | Score | Decision |
| ---- | ------- | ----- | -------- |
| 1 | Guided stage progression + next-action prompts | 14 | Must Have |
| 2 | Fast application capture (manual + URL paste minimal parse) | 14 | Must Have |
| 3 | Email notifications (status change, follow-up due, 7-day stall) | 13 | Must Have |
| 4 | Pipeline board/list with canonical statuses | 13 | Must Have |
| 5 | Status history timeline per application | 11 | Must Have |
| 6 | Follow-up scheduler (simple due-date support) | 9 | Consider for MVP |
| 7 | Basic funnel analytics (`identified->submitted`, `submitted->recruiter_screen`) | 8 | Consider for MVP |
| 8 | Saved views/filters (status/source/date) | 7 | Consider for MVP |
| 9 | Duplicate detection on URL/source | 5 | Defer |
| 10 | Direct source integrations (job board APIs) | 3 | Defer |

---

## Decisions

**Must Have MVP (Primary High OR Top Tier Score):**

- Guided stage progression + next-action prompts (14)
- Fast application capture (manual + URL paste minimal parse) (14)
- Email notifications (status change, follow-up due, 7-day stall) (13)
- Pipeline board/list with canonical statuses (13)
- Status history timeline per application (11)

**Consider for MVP:**

- Follow-up scheduler (simple due-date support) (9)
- Basic funnel analytics (8)
- Saved views/filters (7)

**Defer (Nice-to-Have or Low Strategic Value):**

- Duplicate detection on URL/source (5)
- Direct source integrations (job board APIs) (3)

---

## Feature Notes

- The top five items form the minimum strategic loop: capture -> structure -> act -> remind -> review.
- Follow-up scheduler can be introduced lightly in v1 if implementation velocity allows.
- Analytics should prioritize goal measurement, not broad dashboard scope.

---

_Generated with Whiteport Design Studio framework_  
_Strategic input for UX scenarios and implementation planning_
