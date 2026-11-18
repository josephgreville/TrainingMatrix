# ShiftReady — Hackathon Pitch

## The Problem

Supervisors in a 36-person manufacturing firm need to answer two questions quickly:
1. **Who needs training soon?** — Certificates for must-have safety courses are expiring, and the firm needs to plan renewals before work becomes non‑compliant.
2. **Who can cover a job when someone is away?** — If a colleague is off (ill, on leave), who has the current certificates to step in?

Without a tool, supervisors spend minutes (or hours) manually cross‑referencing spreadsheets: checking who’s absent, who’s qualified, and whose tickets are about to run out. In a fast‑moving production environment, that delay can cause unsafe shortcuts or unplanned downtime.

## What "At Risk" Means

> **"At risk"** means a staff member has a *must‑have* course certificate that is either already expired or will expire within 90 days (by 17 February 2027), **or** they are missing a currently required course for their role entirely.

We treat “nice‑to‑have” courses (C12‑C15) as non‑essential for risk status — they do not count as role gaps. Blank expiry dates (used for courses with no expiry, e.g. Lean basics) are explicitly ignored in the calculation.

This definition was chosen because:
- It focuses on safety‑critical, legally required certifications.
- It gives a 90‑day planning window so renewals can be scheduled before expiry.
- It excludes courses that are optional or have no expiry, keeping the list actionable.

**Verified counts (as of Wed 18 Nov 2026):**
- **11** must‑have certificates are already expired
- **18** must‑have certificates expire within the next 90 days
- **13** staff members have at least one current‑role gap (missing required course)
- **3** staff are absent today (S004, S033, S036)

## What We Built: ShiftReady

ShiftReady is a single‑page dashboard (`index.html`) that reads the challenge's `data.js` and six CSV files. It runs entirely locally — no data leaves the machine.

### Core views

| View | What it shows |
|---|---|
| **At a glance** | Expired certificates, certificates due within 90 days, people with role gaps, cover tasks with 0‑1 available people |
| **Training to plan** | Searchable list of all certificate issues (expired + soon‑to‑expire + role gaps), filterable by name, role, or course |
| **Find cover** | Select a cover task (structural welding, overhead crane, goods‑in/forklift, wiring loom, final inspection, first‑aid); the dashboard lists qualified colleagues who are available *today*, excluding those on absence/leave |
| **Role requirement gaps** | Table of every staff member, their role, and which required courses are missing or expired |

### Key algorithms (embedded in the page script)

- `valid(sid, cid)` — returns `true` if staff `sid` has a current (non‑expired) certificate for course `cid`. Blank expiry dates are treated as “no expiry” and the function returns `false` for the expiry check, which is correct because those courses have no deadline.
- `awayToday` — built from `absence.csv`; staff recorded as absent or on leave on 2026‑11‑18 are excluded from cover candidate lists.
- Cover discovery — for the selected task, every staff member is checked: do they hold *all* required courses current, and are they not away today? Qualified candidates are listed; those who are qualified but away are shown separately; others are shown with a breakdown of what they’re missing.

## How AI Helped

- **Rule‑shaping:** AI helped translate the brief (“see who needs training soon, who can cover”) into the JavaScript logic for validity checks, 90‑day window calculation, and cover filtering.
- **Record synthesis:** AI summarized linked records (e.g., grouping certificates by staff member, connecting role requirements to individual training histories).
- **First dashboard:** AI produced the initial version of the dashboard HTML/JS, which we then refined and tested against the full dataset.

## Where AI Got It Wrong

- **Blank expiry dates:** An initial attempt at counting expiry dates tried to parse blank expiry fields as dates, causing runtime errors. The fix was to add an explicit rule: *if a record has no `expiry_date`, the course has no expiry and should not be counted as expired or “due soon.”* This rule is now hard‑coded in the `valid()` function and the issue‑generation loop.
- **Date‑parsing edge cases:** Early versions mishandled ISO‑string construction for `new Date()`, leading to off‑by‑one errors in the 90‑day window. Using `new Date(d+'T00:00:00')` with a normalized time component resolved it.

## Keeping Data Safe

- **No external transmission:** The dashboard runs entirely in the browser. No staff data is sent to any service or logged externally.
- **Fictional data:** All staff names, addresses, mobile numbers, NI numbers, salaries, and emergency contacts are fictional. The dashboard only ever uses the fields needed for planning: `staff_id`, `name`, `role`, `shift`, `course_id`, `completed_date`, `expiry_date`.
- **Minimal projection:** `data.js` is a sanitized projection of the six CSVs — it deliberately omits all payroll and personal identifiers. The original CSVs remain unchanged on disk.
- **Local‑only deployment:** Run `python -m http.server 8000` from this folder and open `http://localhost:8000`. The server is local; there is no cloud component.

## Business Case

| Benefit | Impact |
|---|---|
| **Faster cover checks** | Supervisors can answer "who can cover?" in seconds instead of minutes of manual spreadsheet work. Estimated time savings: ~5 min per cover request, ~40 hrs/year across 8 shifts/week. |
| **Earlier renewal planning** | The 90‑day “at‑risk” window gives supervisors time to book training before certificates lapse, reducing the risk of non‑compliant work or unsafe shortcuts. |
| **Reduced cognitive load** | One UI replaces a manual cross‑reference of three CSVs (staff, courses, training records). Fewer errors from fatigue or oversight. |
| **Pilot‑ready** | The dashboard is operational immediately; a pilot with 2‑3 supervisors can measure time saved and refine the “at‑risk” threshold before wider rollout. |

**Next step:** Run a pilot with supervisors in the Fabrication and Assembly teams. Track (a) how quickly they find cover, (b) how many renewal actions they initiate, and (c) any feedback on the “at risk” definition. Use those metrics to estimate ROI and plan a connected, approved‑source‑truth version for production.

## Next Steps

1. **Confirm the “at‑risk” rule** with safety leads — we define it as expired or expiring within 90 days for must‑have courses, or missing a required role course.
2. **Pilot the dashboard** with 2‑3 supervisors on Wed 18 Nov 2026; gather correction requests.
3. **Add shift‑to‑task mapping** if the site wants shift‑aware cover (currently shown for context only).
4. **Connect an approved source of truth** (e.g., an LMS or HR database) for future iterations, keeping the local dashboard as the fallback.
5. **Extend the “nice‑to‑have” filter** if the business wants to track optional skills (e.g., mentor/coach, lean basics) separately.
6. **Document the data pipeline** so the six CSVs can be regenerated annually without breaking the dashboard.

## Pitch Summary

> **ShiftReady** turns the hackathon's six CSV files into a supervisor's quick‑look dashboard. It answers two questions that previously required manual spreadsheet work: *who needs training soon* (11 expired + 18 due within 90 days across 13 staff) and *who can cover a job when someone's away* (e.g., 3 qualified people for structural welding today, 11 for goods‑in/forklift). The "at risk" definition is explicit: must‑have certificates expired or expiring within 90 days, or a missing required course. The tool runs locally, uses only planning‑relevant fields, and includes explicit handling for blank expiry dates — a bug we found and fixed during testing. The business case is faster cover checks, earlier renewal planning, and reduced cognitive load for supervisors, with a short pilot as the next step.