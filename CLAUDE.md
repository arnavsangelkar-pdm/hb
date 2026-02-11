# HB Elements — Lead Qualification Dashboard Demo

## Project Overview

A sales demo dashboard for **HB Elements Inc.** (https://www.hbelementsinc.com/) showing a lead qualification system. HB Elements is a B2B manufacturer of decorative exterior PVC millwork and moulding for coastal/high-moisture environments. They sell to architects, contractors, and high-end builders. The company receives a high volume of inbound leads and needs a way to qualify which ones are worth pursuing.

This is a **fully fake demo** — all data is fabricated but realistic for the millwork industry.

## Deployment

- **Live URL**: https://arnavsangelkar-pdm.github.io/hb/
- **GitHub Repo**: https://github.com/arnavsangelkar-pdm/hb
- **Hosting**: GitHub Pages, serving from the `/docs` directory on the `main` branch
- **No build step** — everything is static HTML/CSS/JS

## Project Structure

```
/Users/pdm/Desktop/Demos/HB/
├── CLAUDE.md                 ← this file
├── .gitignore
├── render.yaml               ← Render config (unused, GitHub Pages is primary)
├── Logos and Color/           ← brand assets (source files)
│   ├── HB Logo.png
│   ├── HB Elements Logo (Vertical).png
│   ├── HB Elements Logo (Horizontal).pdf
│   ├── HB Elements Logo (Vertical).pdf
│   └── Screenshot 2026-02-11 at 10.42.37 AM.png  ← brand color reference
└── docs/                     ← served by GitHub Pages
    ├── index.html             ← THE ENTIRE DASHBOARD (single file, ~2200 lines)
    └── images/
        └── hb-logo.png        ← copy of HB Logo.png for web serving
```

## Architecture

Everything is in a **single `docs/index.html`** file — all HTML, CSS (`<style>` block), and JavaScript (`<script>` block) are inline. No build tools, no frameworks, no bundling.

### External CDN Dependencies
- **Chart.js 4.4.1** — doughnut and horizontal bar charts
- **Inter font** — Google Fonts (weights 300–700)
- **Font Awesome 6.5.0** — icons throughout the UI

### Brand Colors

| Token | Hex | Usage |
|-------|-----|-------|
| Primary | `#0066CC` | Buttons, active states, accents |
| Secondary | `#028ED1` | Sidebar highlights, links |
| Grey Light | `#E7E7E7` | Borders, table dividers |
| Grey Medium | `#6D6D6D` | Secondary text |
| Grey Dark | `#3D3D3D` | Body text |
| Sidebar BG | `#1A1D23` | Dark charcoal sidebar |
| Body BG | `#F0F2F5` | Light gray page background |

All colors are defined as CSS custom properties in `:root`.

## Dashboard Components

### Layout
- **Sidebar** (260px, dark) — logo, nav, user avatar ("Jessica Martinez, Sales Director")
- **Header** (64px) — page title, search input, notifications, date range
- **Main content** — scrollable area with all dashboard widgets
- **Detail panel** (500px) — slides in from right when a lead row is clicked

### Widgets
1. **5 KPI Cards** — Total Leads (247), Qualified (83), Conversion Rate (33.6%), Pipeline Value ($4.2M), Avg Response Time (2.4 hrs)
2. **Lead Score Distribution** — Chart.js doughnut (Hot/Warm/Cold) with center text and custom legend
3. **Qualification Pipeline** — Chart.js horizontal bar (New/Contacted/Qualified/Proposal/Won/Lost)
4. **Filter bar** — score tier buttons (All/Hot/Warm/Cold), status dropdown
5. **Leads table** — sortable, filterable, 18 leads with score circles and status badges
6. **Detail panel** — contact info, project details, architecture designs section, score breakdown bars, interaction timeline, action buttons

## Lead Data (18 Fake Leads)

Each lead object has:
- Contact info (name, email, phone, company, role, professionalType)
- Project info (projectType, location, estimatedBudget, timeline, productsInterested, sqFootage)
- `designsShared` — `{ shared: boolean, files: [{ name, type, date }] }`
- `scoreBreakdown` — 7 factors summing to `leadScore`
- `interactions` — array of timeline events (inbound, call, email, meeting)
- `status` — new / contacted / qualified / proposal / won / lost

### Lead Score Breakdown (out of 100)

**Designs Shared is the #1 factor at 30%.**

| Factor | Max | What It Measures |
|--------|-----|-----------------|
| **Designs Shared** | **30** | Whether architecture/CAD files have been shared — biggest qualification signal |
| Project Budget | 15 | Higher budget = higher score |
| Location | 15 | Coastal/Caribbean locations score highest |
| Professional Type | 15 | Architects > Builders > Contractors > Designers > Homeowners |
| Timeline Urgency | 10 | Immediate timelines score higher |
| Product Fit | 10 | How many HB product lines match the project |
| Relationship | 5 | Returning clients and referrals score higher |

### Score Tiers
- **Hot** (70–100): Green — high-value, ready to close
- **Warm** (40–69): Amber — promising, needs nurturing
- **Cold** (0–39): Red — low priority or unqualified

Leads without shared designs are heavily penalized and typically fall into Warm or Cold.

## Interactive Features

- **Sort** — click any sortable column header (Name, Company, Budget, Score, Date)
- **Filter by tier** — All / Hot / Warm / Cold buttons
- **Filter by status** — dropdown (All / New / Contacted / Qualified / Proposal / Won / Lost)
- **Search** — real-time filter by name or company
- **Detail panel** — click any table row to open; close with X, backdrop click, or Escape
- **Animated score bars** — stagger-animate when detail panel opens
- **Load animations** — KPI cards, charts, and table fade in on page load

## Key JavaScript Functions

- `getFilteredLeads()` — applies tier, status, and search filters, then sorts
- `renderTable()` — clears and rebuilds the table tbody from filtered data
- `openDetailPanel(leadId)` / `closeDetailPanel()` — slide panel in/out, render lead details
- `initCharts()` — creates Chart.js doughnut and bar charts
- `attachEventListeners()` — wires up filters, sort, search, panel close
- `animateOnLoad()` — staggered visibility animations

## Workflow

To make changes:
1. Edit `docs/index.html`
2. Open locally in browser to verify (`open docs/index.html`)
3. Commit and push — GitHub Pages auto-deploys in ~1-2 minutes

## Products Referenced in Lead Data

These are real HB Elements product lines used in the fake data:
- PVC Column Wraps
- Exterior Moulding
- Decorative Brackets
- Stain Series Ceiling (24+ colors)
- Pergola Systems
- Trellis Systems
- Soffit Ceiling
- Louvers
- Grilles
- Rafter Tails
