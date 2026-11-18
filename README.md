# ShiftReady

A small local supervisor dashboard for the Training Matrix hackathon brief.

## Open it

From this folder run `python -m http.server 8000`, then open `http://localhost:8000`. The dashboard uses the challenge's fixed date, Wednesday 18 November 2026. It works locally and does not send data to a service.

## What it shows

- Must-have certificates expired by the challenge date and must-have certificates due within 90 days.
- Required role courses that are missing or expired, grouped by person.
- For each cover task, colleagues with all listed certificates current, excluding staff recorded as absent or on leave that day. You can also select someone to exclude from the candidate list.
- Qualified colleagues who are away and other candidates' missing task courses.

“At risk” means a must-have course is expired or expires within 90 days, or a person lacks a current role requirement. The certificate list includes all courses with an expiry date; no-expiry courses such as Lean basics are not treated as expired. Cover availability uses only the supplied absence dates. Shift is shown for context because the data does not link tasks to shifts.

## Pitch notes

- **Problem:** supervisors need a quick, reliable view of certificate risk and cover when a colleague is away.
- **Build and AI:** ShiftReady turns the supplied tables into an at-a-glance risk list and a task-based cover finder. AI helped shape the rule, summarize linked records, and produce the first dashboard. A human should confirm any safety-critical assignment.
- **Testing:** checked all 36 staff and 195 training rows; confirmed 11 expired must-have records, 17 must-have expiry dates within 90 days, and 13 people with at least one current-role gap. Cover counts on 18 Nov: structural welding 3, overhead crane lifts 1, goods in/forklift 11, wiring loom build 4, final inspection 7, first aid 4. Checked that the generated dashboard data omits addresses, dates of birth, mobile numbers, NI numbers, salaries, emergency contacts, absence reasons, and certificate references.
- **What AI got wrong:** an initial date-count attempt tried to parse blank expiry dates and emitted errors. Blank dates belong to courses with no expiry, so the calculation was corrected to ignore them. This is why the no-expiry rule is explicit and counts should be checked against source certificates.
- **Data and safety:** staff records are fictional. The dashboard embeds only the fields needed for planning, and stays on the local machine. Training data may be incomplete or stale; “qualified” means the supplied record lists a current certificate, not that the person has been independently verified or authorized for a particular machine. Absence and shift coverage may also change after the snapshot.
- **Business case:** faster cover checks and earlier renewal planning can reduce avoidable disruption and help supervisors prioritize training. Validate time saved and prevented delays with a pilot before estimating financial returns.
- **Next steps:** confirm expiry and task rules with safety leads; add shifts, skills sign-off, and a changeable date; connect an approved source of truth; pilot with supervisors and gather corrections.

## Data handling

`data.js` is a sanitized, local projection of the six supplied CSVs. It deliberately excludes personal and payroll fields not needed for these decisions. The original challenge CSVs are unchanged.
