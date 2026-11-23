# Max Marsh Design Notebook

**Project:** Ad-Vocates  
**Team members:** Max Marsh and Dylan Shackleford  
**Advisor:** Professor Justin Zhan  

## Week 1: August 24-30

**Entry status:** Reconstructed after the fact.

**Work completed:** I reviewed the Senior Design course structure and requirements and began planning my professional biography and the type of project I wanted to work on.

**Decisions:** I focused my biography on my computer science background and technical experience and began considering a software project that would be practical to design and implement across two semesters.

**Estimated hours:** 1 hour.

**Blockers:** None.

**Evidence:** Professional Biography assignment; biography later committed to the repository.

## Week 2: August 31-September 6

**Entry status:** Reconstructed after the fact.

**Work completed:** I formed a team with Dylan Shackleford and worked with him to decide the direction of the project and the main purpose of Ad-Vocates.

**Decisions:** We chose a privacy-focused browser ad-blocking project with request blocking, user controls, statistics, an allowlist, and filter updates.

**Estimated hours:** 2 hours.

**Blockers:** None.

**Evidence:** [Project Description](../Project-Description.md)

## Week 3: September 7-13

**Entry status:** Reconstructed after the fact.

**Work completed:** I completed and committed my professional biography and continued reviewing the project description and planned feature set with Dylan.

**Decisions:** I focused my role on development and planning, especially the core blocking path, settings, dashboard, storage, and overall implementation strategy.

**Estimated hours:** 3 hours.

**Blockers:** None.

**Evidence:** [Professional Biography](../Professional%20Biography_mxmarsh.txt), [Biography commit](https://github.com/dylan-shack/Ad-Vocates/commit/ed225e8c0f9558f7d87491144744ddf00914899f)

## Week 4: September 14-20

**Entry status:** Reconstructed after the fact.

**Work completed:** I created the team contract and worked with Dylan on project responsibilities, communication expectations, development roles, and overall project scope. I also reviewed the project constraints and updated the project description.

**Decisions:** We divided the primary roles so I would focus on development and planning while Dylan would focus on testing and documentation, while still sharing architecture, reviews, and major decisions.

**Estimated hours:** 4 hours.

**Blockers:** We needed to keep the project scope realistic while still covering privacy, usability, and enough functionality for a senior design project.

**Evidence:** [Team Contract](../Team_Contract.md), [Team contract commit](https://github.com/dylan-shack/Ad-Vocates/commit/fff01f8e), [Project Description](../Project-Description.md)

## Week 5: September 21-27

**Entry status:** Reconstructed after the fact.

**Work completed:** I created the Ad-Vocates user stories and use cases, including the stakeholder map, acceptance-focused stories, and the main blocking and site-recovery flows. Dylan reviewed the work and we discussed the final wording and behavior.

**Decisions:** We included blocking, statistics, allowlisting, automatic filter updates, and a recovery path for websites that break because of filtering. We kept the stories focused on user needs instead of locking the project into one implementation too early.

**Estimated hours:** 5 hours.

**Blockers:** The main challenge was making the stories specific enough to test while avoiding design details that belonged in later assignments.

**Evidence:** [User Stories and Use Cases](../User_Stories.md), [User stories commit](https://github.com/dylan-shack/Ad-Vocates/commit/0131f038)

## Week 6: September 28-October 4

**Entry status:** Reconstructed after the fact.

**Work completed:** I reviewed and collaborated with Dylan on the D0 high-level design. We worked through the component breakdown, responsibilities, data flow, and how the browser request path should interact with settings, local storage, statistics, filter updating, and validation.

**Decisions:** We kept the architecture separated into Dashboard, Settings, Filter Request, Local Storage, Statistic Recorder, Filter Updater, and Filter Validator. We also kept normal browsing-related data local.

**Estimated hours:** 3 hours.

**Blockers:** We had not yet selected the exact browser blocking API, filter source, or final storage technology.

**Evidence:** [D0 High-Level Design](../Design_Diagrams/D0_High_Level_Design.pdf), [D0 Markdown](../Design_Diagrams/D0_High_Level_Design.md)

## Week 7: October 5-11

**Entry status:** Reconstructed after the fact.

**Work completed:** I created and finalized the D1 detailed design with Dylan's review and feedback. I worked on the data model, request-filtering and allowlist algorithms, API contracts, storage records, technology options, and the connections back to the D0 architecture.

**Decisions:** We defined separate records for settings, allowlist entries, filter rules, statistics, and error logs. We planned to keep active filtering data available in memory so request processing would not require a storage lookup for every request. We also kept several technology choices open instead of selecting them without enough investigation.

**Estimated hours:** 6 hours.

**Blockers:** We had not finalized browser storage versus IndexedDB, the exact filter list, or the exact Chromium request-blocking API.

**Evidence:** [D1 Detailed Design](../Design_Diagrams/D1_Detailed_Design.md), [D1 final commit](https://github.com/dylan-shack/Ad-Vocates/commit/a041f867)

## Week 8: October 12-18

**Entry status:** Reconstructed after the fact.

**Work completed:** I reviewed and collaborated with Dylan on D2. I checked the detailed component behavior against the D1 interfaces and helped review failure handling, invalid input, settings recovery, update behavior, and duplicate statistic events.

**Decisions:** We kept failed operations recoverable by preserving previously valid settings or filtering rules. We also made sure repeated or duplicate block events would not cause statistics to be counted more than once.

**Estimated hours:** 3 hours.

**Blockers:** The final browser API and filter source were still open implementation choices.

**Evidence:** [D2 Detailed Design](../Design_Diagrams/D2_Detailed_Design.pdf), [D1 Detailed Design](../Design_Diagrams/D1_Detailed_Design.md)

## Week 9: October 19-25

**Entry status:** Reconstructed after the fact.

**Work completed:** I worked with Dylan to turn the design into an implementation plan. We reviewed the component dependencies, divided the main development work, and checked that the planned milestones and task order matched the architecture.

**Decisions:** We divided implementation into ten main tasks with five primary tasks for each team member. My planned tasks cover Local Storage, Settings and allowlisting, Filter Request, Dashboard, and final UI/documentation work.

**Estimated hours:** 2 hours.

**Blockers:** The implementation plan needed to account for unresolved browser and filter-source choices before the related coding tasks could be completed.

**Evidence:** [Task List](../Tasklist.md), [Planning Package](../Planning/TaskList_Milestones_Timeline_EffortMatrix.pdf)

## Week 10: October 26-November 1

**Entry status:** Reconstructed after the fact.

**Work completed:** I created and revised the Requirements Document and Technical Specification, with Dylan reviewing them against the existing D0, D1, D2, user stories, constraints, and implementation plan. I also organized the files in the repository and revised them after review.

**Decisions:** We consolidated the existing design into stable requirement IDs, measurable non-functional requirements, component interfaces, local data formats, constraints, and a requirement-to-design traceability matrix. We kept unresolved design choices identified instead of treating them as completed decisions.

**Estimated hours:** 6 hours.

**Blockers:** The exact Chromium blocking API, production filter source, and final storage split were still unresolved and needed later investigation.

**Evidence:** [Requirements Document](../Requirements/Requirements%20%26%20Specifications/Requirements_Document.md), [Technical Specification](../Requirements/Requirements%20%26%20Specifications/Technical_Specifications.md), [Requirements revision commit](https://github.com/dylan-shack/Ad-Vocates/commit/94810f82), [Technical specification revision commit](https://github.com/dylan-shack/Ad-Vocates/commit/20cff426)

## Week 11: November 2-8

**Entry status:** Reconstructed after the fact.

**Work completed:** I reviewed Dylan's test plan and checked that the planned tests covered the components and requirements I was responsible for, especially Filter Request, Settings, Local Storage, Dashboard behavior, allowlisting, persistence, and request-filtering performance.

**Decisions:** We kept the tests measurable with controlled inputs, expected results, timing targets, restart checks, and failure cases. We also kept usability testing tied to the user-facing controls defined earlier in the project.

**Estimated hours:** 2 hours.

**Blockers:** None.

**Evidence:** [Test Plan](../Test_Plan.md), [Test plan commit](https://github.com/dylan-shack/Ad-Vocates/commit/ef5b3efbdee50026c729f159fb0ee2f0f98dae1a)

## Week 12: November 9-15

**Entry status:** Reconstructed after the fact.

**Work completed:** I reviewed and collaborated on the ethics and responsible-use statement. I checked the planned data handling, privacy behavior, local storage decisions, filter-source security, logging limits, licensing status, and AI-use disclosure against the rest of our design.

**Decisions:** We kept normal browsing data local, limited error logging, avoided storing unnecessary URLs, and documented unresolved licensing questions instead of assuming a filter source or license.

**Estimated hours:** 2 hours.

**Blockers:** The production filter source, filter-list license, and final project license were still undecided.

**Evidence:** [Ethics Statement](../Ethics_Statement.md), [Ethics statement commit](https://github.com/dylan-shack/Ad-Vocates/commit/fe0c105940b22ec7cc8913d2f9c695d67745f60e)

## Week 13: November 16-22

**Entry status:** Reconstructed after the fact.

**Work completed:** I created the main presentation draft from the existing project documents and worked with Dylan to review it before it was uploaded. I simplified the D0 architecture for a live presentation, summarized project progress, constraints, expected accomplishments, division of work, and the planned Expo demo, and prepared a balanced speaker plan.

**Decisions:** We kept the presentation focused on the existing design instead of adding new project features. We used the D0 architecture as the main live diagram and kept more detailed design information available as backup material.

**Estimated hours:** 5 hours.

**Blockers:** The main unresolved technical issue remained the exact Chromium blocking API and how the selected filter format would map into the request-blocking design.

**Evidence:** [Presentation PowerPoint](../Presentation/Ad-Vocates_Presentation_Draft.pptx), [Presentation PDF](../Presentation/Ad-Vocates_Presentation_Draft.pdf), [Presentation upload commit](https://github.com/dylan-shack/Ad-Vocates/commit/802c8ebfdde6ce0546cdf5ad8dfc63e4eba612eb)

## Week 14: November 23-29

**Status:** Planned work for the upcoming week; not yet completed.

**Planned work:** Complete my design notebook and individual assessment, review the final design report with Dylan, resolve any remaining inconsistencies between the report and project documents, and complete any remaining advisor-assessment requirements.

**Planned decisions:** Confirm the final Fall design baseline and make sure the open Spring implementation questions are clearly documented.

**Hours:** Not yet recorded; excluded from the total.

**Open items:** Design notebook, individual assessment, advisor assessment, and final report review.

## Total Hours

**Total estimated hours for Weeks 1-13: 44 hours.**

## Summary

My work this semester focused mainly on development planning and turning the project idea into detailed requirements and technical design. I created the team contract, user stories and use cases, D1 detailed design, requirements document, technical specification, and presentation draft. I also reviewed and collaborated with Dylan on D0, D2, the implementation plan, test plan, ethics statement, and final design-report material. My main technical focus has been the core request-filtering path, settings and allowlisting, local storage, dashboard behavior, and making sure the design can be implemented and tested in Spring 2027. The main open technical questions are still the exact Chromium request-blocking API, production filter source and format, and final storage split.
