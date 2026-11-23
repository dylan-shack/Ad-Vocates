# Ad-Vocates Final Design Report

## Table of Contents

1. [Team names including the advisor, and a project abstract of at most 400 ASCII characters](#section-1)
2. [Project Description](#section-2)
3. [User Stories and Design Diagrams](#section-3)
4. [Project Tasks and Timeline](#section-4)
5. [ABET Concerns Essay](#section-5)
6. [PPT Slideshow](#section-6)
7. [Self-Assessment Essays](#section-7)
8. [Professional Biographies](#section-8)
9. [Budget](#section-9)
10. [Appendix](#section-10)

<a id="section-1"></a>
## 1. Team names including the advisor, and a project abstract of at most 400 ASCII characters

**Team members:** Dylan Shackleford and Max Marsh  
**Advisor:** Professor Justin Zhan

**Abstract:** Ad-Vocates is a Chromium browser extension that blocks ads and trackers, lets users allow trusted sites, and displays blocked-request counts and estimated data savings. 
Settings and statistics stay local, and filtering rules are updated from approved sources.

<a id="section-2"></a>
## 2. Project Description

Ad-Vocates gives users control over browser filtering through protection settings, an allowlist, and a dashboard showing blocking statistics.

[Full Project Description](Project-Description.md)

<a id="section-3"></a>
## 3. User Stories and Design Diagrams

### User Stories

The user stories cover blocking ads and trackers, viewing statistics, allowing trusted sites, updating filters, and recovering when filtering breaks a website.

[User Stories](User_Stories.md)

### Design Diagrams

- [D0 High-Level Design](Design_Diagrams/D0_High_Level_Design.pdf)
- [D1 Detailed Design](Design_Diagrams/D1_Detailed_Design.pdf)
- [D2 Detailed Design](Design_Diagrams/D2_Detailed_Design.pdf)

### Diagram Conventions

Solid boxes represent components built by the team, and circles represent external dependencies. Arrows in the D0 diagram represent interfaces. Arrows in the data-flow diagram show the data being passed, with labels identifying its form.

In the data model, boxes represent stored entities, PK identifies primary keys, and FK identifies foreign keys. Relationship notation shows how records relate, including one-to-many relationships between settings and allowlist entries or filter rules. Component names remain consistent across the design documents.

### Components

| Component | Purpose |
|---|---|
| Dashboard | Displays statistics and protection controls. |
| Settings | Manages protection settings and allowlist changes. |
| Filter Request | Determines whether requests should be allowed or blocked. |
| Local Storage | Stores settings, filtering data, statistics, and error records locally. |
| Statistic Recorder | Records blocked-request counts and estimated data savings. |
| Filter Updater | Retrieves filtering data from approved sources. |
| Filter Validator | Checks downloaded filtering data before activation. |

### Requirements, Specification, and Test Plan

- [Requirements Document](Requirements/Requirements%20%26%20Specifications/Requirements_Document.md)
- [Technical Specification](Requirements/Requirements%20%26%20Specifications/Technical_Specifications.md)
- [Test Plan](Test_Plan.md)

### Traceability Matrix

The following matrix connects each requirement and inherited constraint to its design components and planned test cases. Test procedures and expected results are defined in the Test Plan.

| Requirement | Design components | Test cases |
|---|---|---|
| REQ-01 | Filter Request | TC-001, TC-018, TC-027 |
| REQ-02 | Settings, Filter Request | TC-002, TC-003, TC-017 |
| REQ-03 | Settings, Local Storage | TC-003, TC-022, TC-017, TC-035 |
| REQ-04 | Dashboard, Settings, Filter Request | TC-004, TC-025, TC-013 |
| REQ-05 | Filter Request, Statistic Recorder, Dashboard, Local Storage | TC-005, TC-026, TC-027 |
| REQ-06 | Statistic Recorder, Dashboard, Local Storage | TC-005, TC-006, TC-016 |
| REQ-07 | Filter Updater, Filter Validator, Settings, Filter Request | TC-007, TC-008, TC-015 |
| REQ-08 | Filter Updater, Filter Validator, Settings, Local Storage | TC-008, TC-015, TC-032 |
| REQ-09 | Dashboard, Filter Updater | TC-009 |
| REQ-10 | Settings, Local Storage, Statistic Recorder | TC-010, TC-034, TC-035 |
| NFR-01 | Filter Request | TC-011, TC-036, TC-014 |
| NFR-02 | Filter Updater, Filter Validator, Local Storage | TC-008, TC-015, TC-012 |
| NFR-03 | Dashboard, Settings | TC-013, TC-017, TC-016 |
| NFR-04 | Filter Request, Filter Updater, Filter Validator, Settings, Local Storage | TC-032, TC-010, TC-014 |
| CON-01 | Filter Updater, Filter Validator, Local Storage | TC-015, TC-012 |
| CON-02 | Dashboard, Settings, Statistic Recorder | TC-016, TC-013, TC-017 |
| CON-03 | Dashboard, Settings, Filter Request | TC-017 |

### Readiness

The least certain part of the design is how the Filter Request component will use Chromium's request-blocking API and represent rules from approved filter sources. 
During IM01-IM02 in January, we plan to build a small prototype that blocks matching requests, allows trusted sites, applies updated rules, and checks how blocking events can support statistics. 
The results will guide updates to the interfaces, rule format, and related tests before the main implementation continues.

<a id="section-4"></a>
## 4. Project Tasks and Timeline

The implementation plan identifies the project tasks, ownership, milestones, and planned effort.

- [Task List](Tasklist.md)
- [Tasks, Milestones, Timeline, and Effort Matrix](Planning/TaskList_Milestones_Timeline_EffortMatrix.pdf)

<a id="section-5"></a>
## 5. ABET Concerns Essay

The essay discusses security, ethical transparency, and the effect of filtering on website usability.

[ABET Concerns Essay](Constraints_Essay.md)

<a id="section-6"></a>
## 6. PPT Slideshow

- [PowerPoint Slideshow](Presentation/Ad-Vocates_Presentation_Draft.pptx)
- [Presentation PDF](Presentation/Ad-Vocates_Presentation_Draft.pdf)

<a id="section-7"></a>
## 7. Self-Assessment Essays



<a id="section-8"></a>
## 8. Professional Biographies

- [Dylan Shackleford](ProfBiography_shacklds.md)
- [Max Marsh](Professional%20Biography_mxmarsh.txt)

<a id="section-9"></a>
## 9. Budget

There have been no project expenses or donated items to date. No future expenses are currently planned.

<a id="section-10"></a>
## 10. Appendix

### References

- [Project Description](Project-Description.md)
- [User Stories](User_Stories.md)
- [D0 High-Level Design](Design_Diagrams/D0_High_Level_Design.pdf)
- [D1 Detailed Design](Design_Diagrams/D1_Detailed_Design.pdf)
- [D2 Detailed Design](Design_Diagrams/D2_Detailed_Design.pdf)
- [Requirements Document](Requirements/Requirements%20%26%20Specifications/Requirements_Document.md)
- [Technical Specification](Requirements/Requirements%20%26%20Specifications/Technical_Specifications.md)
- [Test Plan](Test_Plan.md)
- [Project Planning](Planning/TaskList_Milestones_Timeline_EffortMatrix.pdf)
- [ABET Concerns Essay](Constraints_Essay.md)
- [Team Contract](Team_Contract.md)
- [Ethics Statement](Ethics_Statement.md)

### Repository

[Ad-Vocates Repository](https://github.com/dylan-shack/Ad-Vocates)


### Individual Effort

- [Dylan Shackleford's Design Notebook](notebooks/shackleford_notebook.md) — 47 estimated hours through Week 13.
- [Commit History](https://github.com/dylan-shack/Ad-Vocates/commits/main/)

