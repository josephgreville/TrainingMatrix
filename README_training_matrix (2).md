# Training matrix

## The scenario

A manufacturing firm has 36 staff, and a lot of them need certificates to do their jobs safely: manual handling, forklift, fire marshal, welding and more. Certificates run out. Supervisors have to plan ahead, and when someone is away they need to know who else can do the job.

Today it all lives in spreadsheets. A supervisor wants to ask "if this person is off, who can do the job?" and get an answer in seconds.

All staff and records are invented for this challenge. "Today" is Wednesday 18 November 2026.

## The data

| File | What it tells you |
|---|---|
| staff.csv | Everyone who works there, with their role, department and shift |
| courses.csv | Every course, whether it is must-have or nice-to-have, and how long a certificate lasts |
| training_records.csv | Who completed which course, and when it expires |
| absence.csv | Who is away, and when |
| role_requirements.csv | The courses each role needs |
| tasks_needing_cover.csv | Jobs that need covering, and the courses each one requires |

Files link up through staff_id and course_id.

## What to show at the end

Every team gives a 5-minute pitch using this data.

**Must have**
- Who has a certificate that has run out, or is about to
- Who is missing a course their role needs
- For a job that needs covering, who is qualified and who is available

**Nice to have**
- A view a supervisor can read in a minute, such as a grid of staff against courses
- Flags for data problems, and how you decided to handle each
- Something the supervisors could use without help

**Stretch**
- Find jobs where only one person, or nobody, can cover
- Show what changes if "today" changes
- A one-page summary a supervisor could print and pin up
