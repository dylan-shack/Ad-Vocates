# Dylan Shackleford Design Notebook

**Project:** Ad-Vocates  
**Team members:** Dylan Shackleford and Max Marsh  
**Advisor:** Professor Justin Zhan  

## Week 1: August 24-30

**Entry status:** Reconstructed after the fact.

**Work completed:** I reviewed the course syllabus and layout and started my professional biography. 

**Decisions:** I focused the biography on my computer science education and programming experience through my co-ops.

**Estimated hours:** 1 hour.

**Blockers:** None

**Evidence:** Professional Biography assignment; biography uploaded during Week 3.

## Week 2: August 31-September 6

**Entry status:** Reconstructed after the fact.

**Work completed:** I finished my professional biography. I formed a team with Max Marsh and began the project description work with Max and we decided the direction of the project.

**Decisions:** We chose an ad-blocking project with privacy protection, controls, and easily-understandable statistics.

**Estimated hours:** 2 hours.

**Blockers:** None

**Evidence:** [Professional Biography](../ProfBiography_shacklds.md), [Project Description](../Project-Description.md)

## Week 3: September 7-13

**Entry status:** Reconstructed after the fact.

**Work completed:** I added and revised the project description to the repo. Defined the intended users and described the foundation of the extension, including planned blocking, dashboard, settings, allowlist, and filter-update features.

**Decisions:** We decided users should be able to allow trusted websites and view blocking statistics easily, and use the extension easily without needing to configure or manage filter-rules themselves.

**Estimated hours:** 3 hours.

**Blockers:** None

**Evidence:** [Project description commit](https://github.com/dylan-shack/Ad-Vocates/commit/74d66591098533ae29cd25f8cc4cb9c7e71ebcbd), [description revision](https://github.com/dylan-shack/Ad-Vocates/commit/6b6b51267984b1d51b539a122662702a52ce4330)

## Week 4: September 14-20

**Entry status:** Reconstructed after the fact.

**Work completed:** Worked on constraints essay and reviewed and committed the team contract. Documented security, ethical, social concerns, team responsibilities and communication expectations.

**Decisions:** We decided to minimize collected data, verify filter sources, and label data savings as estimates.

**Estimated hours:** 4 hours.

**Blockers:** A concern was balancing blocking and privacy with website usability. The constraints essay identified user controls as the way to address that tradeoff.

**Evidence:** [Constraints essay commit](https://github.com/dylan-shack/Ad-Vocates/commit/52f21c1a4fdf976dcc493fb57558571429067cfb), [signed team contract](https://github.com/dylan-shack/Ad-Vocates/commit/c96c1f6ca401de2c93956dc81514aa71ce11d9cc)

## Week 5: September 21-27

**Entry status:** Reconstructed after the fact.

**Work completed:** Reviewed Max's user stories and use cases. 

**Decisions:** We included website recovery alongside blocking and statistics. We used acceptance criteria that describe results we can check, such as rejecting invalid input and keeping valid filters after a failed update.

**Estimated hours:** 2 hours.

**Blockers:** None

**Evidence:** [User Stories and Use Cases](../User_Stories.md)

## Week 6: September 28-October 4

**Entry status:** Reconstructed after the fact.

**Work completed:** Created and revised the D0 high-level design after feedback from Max. Created component and data-flow diagrams, documented interfaces and responsibilities, and organized the files in the repo.

**Decisions:** We chose a layered structure for the Dashboard, application components, and Local Storage. Filter downloading was separated from validation and we kept filtering and statistics local. 

**Estimated hours:** 6 hours.

**Blockers:** None

**Evidence:** [D0 draft commit](https://github.com/dylan-shack/Ad-Vocates/commit/d10ad545674c361c33121949c9c922dfd80741bf), [D0 final Markdown commit](https://github.com/dylan-shack/Ad-Vocates/commit/132953b66fb076f3c3e52b352889b16d63e5dcfe)

## Week 7: October 5-11

**Entry status:** Reconstructed after the fact.

**Work completed:** Reviewed and worked with Max on D1. Compared the data model, request-filtering algorithm, allowlist behavior and other parts with the D0 architecture.

**Decisions:** We chose local object-based storage with separate records for settings, allowlist entries, filter rules, statistics, and errors. We planned to load and organize rules in memory instead of reading them from storage for every request.

**Estimated hours:** 3 hours.

**Blockers:** Did not decide local-storage platform, browser storage and IndexedDB were the two options we were deciding between

**Evidence:** [D1 Detailed Design](../Design_Diagrams/D1_Detailed_Design.md)

## Week 8: October 12-18

**Entry status:** Reconstructed after the fact.

**Work completed:** Created and revised D2 with Max's feedback on the architecture. Worked through detailed component behavior, error handling, and made sure it was consistent with prior design docs.

**Decisions:** We decided failed changes should preserve valid settings and filters. We defined how invalid input and failed updates should be handled and made sure duplicate events would not be counted twice.

**Estimated hours:** 6 hours.

**Blockers:** None

**Evidence:** [D2 draft](https://github.com/dylan-shack/Ad-Vocates/commit/e34621728646e5f5c0b644a61910e1beb2a9523e), [D2 revision](https://github.com/dylan-shack/Ad-Vocates/commit/dcc344183eacd29115637f77fc70260fa22c7f3a)

## Week 9: October 19-25

**Entry status:** Reconstructed after the fact.

**Work completed:** I reviewed the design docs with Max and we discussed how to divide it into implementation tasks for the planning assignment. We had to consider the dependencies between components when creating our plans.

**Decisions:** We organized implementation around the components and planned browser and filter-source investigation early. We also allowed time to test the components together after development.

**Estimated hours:** 2 hours.

**Blockers:** We did not decide on browser blocking API and filter sources. These needed investigation tasks in the implementation plan.

**Evidence:** [Task List](../Tasklist.md), [Planning Package](../Planning/TaskList_Milestones_Timeline_EffortMatrix.pdf)

## Week 10: October 26-November 1

**Entry status:** Reconstructed after the fact.

**Work completed:** Created and uploaded the task list, milestones, timeline, and effort matrix. Reviewed and worked with Max on the requirements and technical specification, comparing them with the design and planned responsibilities.

**Decisions:** We divided implementation into ten tasks, with five primary tasks per member. My tasks covered browser compatibility, filter validation, filter updates, statistics, and integration testing. We connected measurable requirements to components and planned tests.

**Estimated hours:** 5 hours.

**Blockers:** The Chromium blocking API, filter list, and final storage weren't decided. We planned to investigate these choices before completing the related implementation.

**Evidence:** [Planning commit](https://github.com/dylan-shack/Ad-Vocates/commit/c5f33b768db3ff4c8ff3aaac6330fa20ebc572b1), [Requirements](../Requirements/Requirements%20%26%20Specifications/Requirements_Document.md), [Technical Specification](../Requirements/Requirements%20%26%20Specifications/Technical_Specifications.md)

## Week 11: November 2-8

**Entry status:** Reconstructed after the fact.

**Work completed:** Created the test and validation plan. Defined test cases, setup, expected results, success criteria, and testing schedule. 

**Decisions:** I used controlled inputs and measurable expected results, including checks that settings and filters survive failures. I planned usability testing with five users and separate timing for each task. I also included checks that savings are labeled as estimates.

**Estimated hours:** 5 hours.

**Blockers:** None

**Evidence:** [Test plan commit](https://github.com/dylan-shack/Ad-Vocates/commit/ef5b3efbdee50026c729f159fb0ee2f0f98dae1a)

## Week 12: November 9-15

**Entry status:** Reconstructed after the fact.

**Work completed:** Created the ethics and responsible-use statement. Reviewed the data collected by each component, storage and retention, security assumptions, licensing status, and created AI-use disclosure.

**Decisions:** We kept request data temporary and statistics local, excluded URLs from error logs, and limited log retention. I documented filter-source licensing as pending and explained how AI-assisted drafts were reviewed and rewritten against our design.

**Estimated hours:** 3 hours.

**Blockers:** The filter source and its license were not decided. The project license is also undecided.

**Evidence:** [Ethics statement commit](https://github.com/dylan-shack/Ad-Vocates/commit/fe0c105940b22ec7cc8913d2f9c695d67745f60e)

## Week 13: November 16-22

**Entry status:** Reconstructed after the fact.

**Work completed:** Reviewed and updated the presentation Max created and compared its simplified architecture with D0. Uploaded and organized the presentation files. Began assembling the final design report with document links, component descriptions, conventions, a traceability matrix, and a readiness statement.

**Decisions:** We identified the Chromium blocking API and rule format as the main unresolved design question and planned an early prototype to investigate them.

**Estimated hours:** 5 hours.

**Blockers:** Final-report review identified a missing TC-009 for REQ-09.

**Evidence:** [Presentation upload](https://github.com/dylan-shack/Ad-Vocates/commit/802c8ebfdde6ce0546cdf5ad8dfc63e4eba612eb), final README working draft.

## Week 14: November 23-29

**Status:** Planned work for the upcoming week; not yet completed.

**Planned work:** Finish the notebook, individual assessment, and design report, and get the signed advisor assessment.

**Planned decisions:** Resolve any remaining differences between the report, design documents, and test plan.

**Hours:** Not yet recorded; excluded from the total.

**Open items:** Advisor assessment, individual records, and final report.

## Total Hours

**Total estimated hours for Weeks 1-13: 47 hours.**

## Summary

My work this semester focused on documenting the project and turning its goals into a design and implementation plan. I contributed the project description, constraints essay, D0 and D2 work, planning package, test plan, ethics statement, presentation work, and final-report preparation. I also reviewed Max's deliverables and collaborated on architecture and requirements. The project now has defined components, interfaces, responsibilities, and planned tests. We still need to confirm how the Chromium blocking API and filter format will work with our design before the main implementation.
