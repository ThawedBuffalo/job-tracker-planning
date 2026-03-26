# Logical View Map — Scenario 01: Priya's Core Pipeline Momentum

**Confirmed:** 2026-03-26
**Device:** Fully Responsive (375px–1920px+)
**Fidelity:** Generic Gray Model

---

## View Mapping

| Build Order | Scenario Step | Logical View | HTML File | Internal States |
|-------------|--------------|--------------|-----------|-----------------|
| 1 | 1.1 Home | Home / Sign In | `home.html` | Single state (sign-in form) |
| 2 | 1.2 Dashboard | Dashboard | `dashboard.html` | Empty state · Populated state |
| 3 | 1.3 Add Application | Add Application | `add-application.html` | Manual entry · URL paste |
| 4 | 1.4 Confirm Saved | Confirm Saved | `confirm-saved.html` | Single state + inline "add another" prompt |

---

## Notes

- All 4 steps map to unique logical views — no shared pages or inheritance
- Internal states handled via JS within each HTML file (not separate files)
- Build order is linear: Home feeds Dashboard feeds Add Application feeds Confirm Saved
- Dashboard requires demo data loaded from `data/demo-data.json` (5 applications)

---

## Build Status

| Logical View | Status |
|-------------|--------|
| home.html | not started |
| dashboard.html | not started |
| add-application.html | not started |
| confirm-saved.html | not started |
