# Ad-Vocates Run Sheet

## Speaking Roles

- Max Marsh: introduction, team, system design, project progress, and division of work.
- Dylan Shackleford: abstract, constraints, end-of-term accomplishments, and expo demo.
- Timekeeper: Dylan Shackleford.
- Planned presentation length: 8 minutes.

## Slide Schedule

| Slide | Speaker | Start | Duration | Hand-off line |
|---|---|---|---|---|
| 1. Ad-Vocates | Max | 0:00 | 0:30 | “First, here is our team and advisor.” |
| 2. Team & Advisor | Max | 0:30 | 0:20 | “Dylan will summarize what the extension does.” |
| 3. Project Abstract | Dylan | 0:50 | 0:55 | “Max will explain how our user stories connect to the design.” |
| 4. User Stories → System Design | Max | 1:45 | 1:25 | “Dylan will cover the main constraints behind these decisions.” |
| 5. Major Project Constraints | Dylan | 3:10 | 0:50 | “Max will review our progress so far.” |
| 6. Project Progress | Max | 4:00 | 0:55 | “Dylan will cover what we plan to finish by the end of the term.” |
| 7. Expected End-of-Term Accomplishments | Dylan | 4:55 | 1:00 | “Max will explain how we divided the implementation work.” |
| 8. Division of Work | Max | 5:55 | 0:55 | “Dylan will walk through our planned expo demo.” |
| 9. Expected Expo Demo | Dylan | 6:50 | 1:10 | “That concludes our presentation. We’re ready for questions.” |

## Anticipated Questions

| Question | Owner | Short answer |
|---|---|---|
| Why build this when existing ad blockers are available? | Max | Our goal is to give users straightforward control over blocking, a clear view of what is being blocked, and an easy way to restore sites when something breaks. We also want to put an emphasis on privacy with local data storage, and try to build a strong ad-blocker around Chromium, which has recently had updates that make popular blockers less effective. |
| How will you verify that the extension actually blocks requests? | Dylan | We plan to check browser network activity against expected results from controlled test cases. |
| Can the extension block every ad or tracker? | Max | No, the coverage depends on the active rules and browser capabilities. Requests that are not covered could still get through. |
| How will you determine whether the extension slows down browsing? | Dylan | We will need to compare the same browsing scenarios with protection enabled and disabled. |
| Could a valid filter update still break legitimate websites? | Dylan | Yes. Rule validation cannot guarantee that every rule is appropriate for every site. |
| What is the biggest implementation uncertainty? | Dylan | How our planned filtering behavior maps to Chromium’s blocking API and rule limits. That research may require adjustments to the design. |

## Full-Group Timed Rehearsal Log

- Date: 11/20/2026
- Participants: Max Marsh and Dylan Shackleford
- Timekeeper: Dylan Shackleford
- Actual total time: 10:46

| Section | Planned Time | Actual Time |
|---|---|---|
| Introduction and team — Slides 1–2 | 0:50 | 1:02 |
| Project abstract — Slide 3 | 0:55 | 1:18 |
| User stories and system design — Slide 4 | 1:25 | 1:58 |
| Constraints — Slide 5 | 0:50 | 1:40 |
| Project progress — Slide 6 | 0:55 | 0:50 |
| End-of-term accomplishments — Slide 7 | 1:00 | 1:30 |
| Division of work — Slide 8 | 0:55 | 1:03 |
| Expo demo — Slide 9 | 1:10 | 1:25 |
| Total | 8:00 | 10:46 |

