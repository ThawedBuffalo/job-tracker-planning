# Scenario 01: Priya's Core Pipeline Momentum — Prototype Roadmap

**Scenario**: Priya's Core Pipeline Momentum
**Pages**: Home through Confirm Saved
**Device Compatibility**: Fully Responsive (375px–1920px+)
**Design Fidelity**: Generic Gray Model (Wireframe)
**Last Updated**: 2026-03-26

---

## 🎯 Scenario Overview

**User Journey**: Priya opens the app and lands on the Home page, navigates to her Dashboard to review her active pipeline, adds a new job application via the Add Application form, and lands on the Confirm Saved page after a successful save.

**Pages in this Scenario**:
1. **1.1 Home** — Entry point, CTA to enter the app
2. **1.2 Dashboard** — Active pipeline overview with status-grouped applications
3. **1.3 Add Application** — Form to capture a new job application
4. **1.4 Confirm Saved** — Success confirmation after saving an application

---

## 📱 Device Compatibility

**Type**: Fully Responsive

**Reasoning**:
job-tracker is a Flutter Web + mobile-web product used across devices by job seekers. Priya may check her pipeline from her phone during commutes and add applications from a desktop. Full responsiveness is required to validate the layout across all form factors.

**Test Viewports**:
- iPhone SE (375px × 667px) — Smallest common mobile size
- iPhone 14 Pro (393px × 852px) — Standard mobile size
- iPad (768px × 1024px) — Tablet breakpoint
- MacBook 13" (1280px × 800px) — Standard desktop
- Full HD (1920px × 1080px) — Wide desktop

**Tailwind Breakpoints**:
```html
<!-- sm: 640px | md: 768px | lg: 1024px | xl: 1280px | 2xl: 1536px -->
<!-- Mobile-first: base styles for mobile, override at md/lg/xl -->
```

**Optimization Strategy**:
- ✅ Mobile-first layout (stack → row as viewport grows)
- ✅ Touch-friendly tap targets (min 44px)
- ✅ Readable type at all sizes
- ✅ Responsive grid for dashboard pipeline columns
- ❌ No hover-only interactions (must work on touch)
- ❌ No fixed-width layouts

---

## 🎨 Design Fidelity

**Approach**: Generic Gray Model

- Grayscale color palette (grays + one accent blue for primary actions)
- Tailwind CSS CDN (no custom design tokens)
- Focus on layout, flow, and functionality — not visual polish
- Placeholder text for icons (text labels used instead of SVG icons where possible)

---

## 📁 Folder Structure

**HTML Files** (root level — double-click to open in browser):
```
home.html          ← 1.1 Home
dashboard.html     ← 1.2 Dashboard
add-application.html  ← 1.3 Add Application
confirm-saved.html    ← 1.4 Confirm Saved
```

**Supporting Folders**:
- `data/demo-data.json` — Priya's demo application dataset (auto-loads on first use)
- `work/` — Planning files (work.yaml per page, filled during development)
- `stories/` — Section implementation guides (created just-in-time)
- `shared/` — Shared JS utilities (data helpers, navigation, sessionStorage)
- `components/` — Reusable UI components
- `pages/` — Page-specific scripts (only if complex)
- `assets/` — Images, icons

---

## 🚀 Quick Start

### For Testing
1. **Open** `home.html` (double-click in Finder)
2. **Demo data prompt** → Click YES to load Priya's pipeline data
3. **Navigate** through the flow: Home → Dashboard → Add Application → Confirm Saved
4. **Data persists** across pages via sessionStorage

### For Stakeholders
1. Open `home.html` in any browser
2. Accept demo data when prompted
3. Walk through the full Priya journey
4. Share feedback page by page

---

## 📋 Build Status

| Page | File | Status | Story Files |
|------|------|--------|-------------|
| 1.1 Home | home.html | not started | — |
| 1.2 Dashboard | dashboard.html | not started | — |
| 1.3 Add Application | add-application.html | not started | — |
| 1.4 Confirm Saved | confirm-saved.html | not started | — |
