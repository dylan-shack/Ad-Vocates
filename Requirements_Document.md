# Ad-Vocates Requirements Document

**Project:** Ad-Vocates  
**Team Members:** Max Marsh, Dylan Shackleford  
**Document Version:** 1.2 
**Date:** October 30, 2026

## 1. Version and Change Log

| Version | Date | Change | Author |
|---|---|---|---|
| 0.1 | 2026-10-30 | Consolidated Week 4 user stories, project constraints, D0-D2 design material, and the Week 8 implementation plan into one requirements draft. | Max Marsh, Dylan Shackleford |
| 1.0 | 2026-10-30 | Established stable requirement IDs, measurable verification conditions, MoSCoW priorities, inherited constraints, assumptions, and open questions for the submission baseline. | Max Marsh, Dylan Shackleford |

Requirement IDs in this document are stable. If a requirement is later removed, its ID will be marked **Withdrawn** in the change log and will not be reused for a different requirement.

## 2. Purpose and Scope

Ad-Vocates is a downloadable, privacy-focused browser ad blocker intended to reduce advertisements, pop-ups, trackers, and suspicious web requests while giving users understandable control over protection behavior. The system will provide a dashboard for protection controls and summary statistics, allow trusted sites to bypass blocking, maintain filtering rules from approved external sources, and store required settings and statistics locally.

The initial release is scoped as a local browser extension. It does not require a cloud backend or user account. The current design is organized around the D0 components **Dashboard**, **Settings**, **Filter Request**, **Local Storage**, **Statistic Recorder**, **Filter Updater**, and **Filter Validator**. The detailed data model and internal interface definitions are documented in D1, and edge cases and final design traceability are documented in D2.

This document defines **what** the system is required to do, the measurable quality levels it must meet, the users and stakeholders it serves, and the inherited project constraints. Implementation details are defined separately in `Technical_Specification.md`.

## 3. Stakeholders and User Goals

The stakeholder set is inherited from `User_Stories.md`.

| Stakeholder | Type | User Goal |
|---|---|---|
| Privacy-conscious web user | Primary | Block known advertising and tracking requests so fewer third parties can track browsing activity and less unwanted content loads. |
| Bandwidth-conscious web user | Primary | See blocked-request counts and an estimated amount of data prevented from loading so the effect of Ad-Vocates is understandable. |
| Ad-Vocates maintainer | Secondary | Update filtering rules from approved sources without requiring users to reinstall the extension. |
| User who depends on an important website or accessibility feature | Hidden | Recover when filtering breaks a site without disabling protection everywhere. |
| Website operator | Hidden | Avoid unnecessary breakage caused by incorrectly blocked legitimate resources. |

The Week 4 user stories remain the source stories for the requirements below:

- **US-01:** Block known advertising and tracking requests.
- **US-02:** Show blocked requests and estimated data savings.
- **US-03:** Allow trusted websites to bypass blocking.
- **US-04:** Update filtering rules from approved sources without reinstalling.
- **US-05:** Recover when filtering breaks an important website or feature.

## 4. Glossary

| Term / Role | Definition |
|---|---|
| Ad-Vocates | The privacy-focused browser extension being developed by the team. |
| Privacy-conscious web user | A primary user who wants fewer advertising, tracking, and unnecessary third-party requests while browsing. |
| Bandwidth-conscious web user | A primary user who wants to reduce unnecessary traffic and understand the estimated data impact of blocking. |
| Ad-Vocates maintainer | A team member responsible for filter sources, testing, fixes, and project maintenance. |
| Website operator | A person or organization responsible for a website that may be affected by blocking behavior. |
| Dashboard | The user-facing component that displays statistics, protection state, filter-update status, and controls. |
| Settings | The component that manages protection state and allowlist changes. |
| Filter Request | The component that decides whether a browser resource request is allowed or blocked. |
| Local Storage | Browser-local storage used for settings, allowlist entries, filter rules or filter metadata, statistics, and error records. |
| Statistic Recorder | The component that records blocked-request totals and estimated data savings. |
| Filter Updater | The component that retrieves updated filtering data from an approved source. |
| Filter Validator | The component that checks downloaded filter data before it is activated. |
| Browser request | A request made by the browser to load a resource such as a script, image, frame, or stylesheet. |
| Filter rule | A rule used to identify advertising, tracking, or other requests that should be blocked. |
| Filter list | A maintained collection of filter rules obtained from an approved source. |
| Approved source | A filter-list source accepted by the team after compatibility, licensing, and security review. |
| Allowlist | A set of trusted website hostnames that bypass normal blocking behavior. |
| Matched request | A browser request that matches an active blocking rule. |
| Resource type | The browser category of a request, such as script, image, frame, or stylesheet. |
| Estimated data saved | An approximate number of bytes that Ad-Vocates estimates were prevented from loading because requests were blocked. |
| Protection state | Whether Ad-Vocates blocking is globally enabled or disabled. |
| Filter refresh | The process of obtaining, validating, and activating a newer filter list. |
| Error log | A local record of a recoverable error such as a storage or filter-update failure. |

## 5. Functional Requirements

| ID | Requirement | MoSCoW | Source Story | Verification Method |
|---|---|---|---|---|
| **REQ-01** | The system shall block a browser request when protection is enabled, the page hostname is not allowlisted, and the request matches an active blocking rule; in a test set of 20 known matching requests, all 20 shall return a `BLOCK` decision. | Must | US-01 | Automated functional test using known matching URLs and the `evaluateRequest` result. |
| **REQ-02** | The system shall bypass normal blocking for an allowlisted hostname; in a test set of 10 requests that would otherwise be blocked, all 10 shall be allowed when the page hostname is allowlisted. | Must | US-03, US-05 | Automated allowlist-bypass test. |
| **REQ-03** | The system shall allow a user to add and remove a valid normalized hostname from the allowlist, reject empty or invalid hostnames, and preserve the resulting allowlist after the extension is reloaded. | Must | US-03, US-05 | UI test plus storage reload test. |
| **REQ-04** | The system shall allow a user to enable or disable global protection, and when protection is disabled, 20 requests that otherwise match active blocking rules shall be allowed without a blocking decision. | Must | US-01, US-05 | UI/state test followed by 20 known matching requests. |
| **REQ-05** | The system shall record each blocked request exactly once and update the displayed blocked-request total within 1 second of the recorded block event. | Must | US-02 | Integration test comparing generated block events with stored and displayed totals. |
| **REQ-06** | The system shall maintain an estimated byte total for blocked requests, display the value on the Dashboard, and label the value as an estimate; the displayed total shall update within 1 second after a recorded block event. | Should | US-02 | Integration/UI test and label inspection. |
| **REQ-07** | The system shall refresh filtering data from an approved source without requiring extension reinstallation, and a successful refresh shall make the validated rule set available to Filter Request during the same extension session. | Should | US-04 | Controlled filter-refresh test using a known valid source snapshot. |
| **REQ-08** | The system shall validate downloaded filter data before activation and shall keep the previously validated rule set active if download or validation fails. | Should | US-04, US-05 | Failure-injection test using unreachable and malformed filter inputs. |
| **REQ-09** | The system shall display the most recent filter-update status and a user-readable failure status when a filter refresh cannot be completed. | Could | US-04 | UI test using successful and failed refresh cases. |
| **REQ-10** | The system shall preserve protection state, allowlist entries, active filter metadata, and summary statistics across a browser or extension restart. | Should | US-02, US-03, US-04 | Persistence test across 10 extension/browser restart cycles. |

## 6. Non-Functional Requirements

| ID | Requirement | MoSCoW | Source Story | Verification Method |
|---|---|---|---|---|
| **NFR-01** | The system shall complete the request-filtering decision within **50 ms for at least 95% of 1,000 synthetic requests** while approximately **10,000 active rules** are loaded on the team's baseline development computer. | Should | US-01 | Timed performance test with 1,000 requests and approximately 10,000 rules. |
| **NFR-02** | The system shall protect locally stored browsing-related data by using **HTTPS for 100% of configured remote filter sources**, rejecting filter data that fails validation, and transmitting **zero user browsing-history or request-URL records to non-filter-source remote services** during network inspection tests. | Must | US-01, US-04 | Configuration review, validation tests, and network inspection. |
| **NFR-03** | At least **4 of 5 test users** shall be able to complete each of these tasks without assistance in **2 minutes or less per task**( This will most likely decrease as the progrma gets better): toggle protection, add/remove an allowlist entry, and locate the blocking statistics. | Should | US-02, US-03, US-05 | Moderated usability test with five participants and timed task completion. |
| **NFR-04** | The system shall complete **1,000 sequential request evaluations without crashing**, retain the previous valid filter set in **10 of 10 simulated update failures**, and preserve saved settings in **10 of 10 restart tests**. | Must | US-04, US-05 | Reliability stress test, failure-injection test, and restart persistence test. |

## 7. MoSCoW Summary

### Must

- REQ-01 - Block requests that match active rules.
- REQ-02 - Bypass blocking for allowlisted sites.
- REQ-03 - Add, remove, validate, and persist allowlist entries.
- REQ-04 - Enable or disable global protection.
- REQ-05 - Count blocked requests exactly once and update the Dashboard.
- NFR-02 - Enforce filter-source security and local privacy behavior.
- NFR-04 - Meet the defined reliability and recovery thresholds.

### Should

- REQ-06 - Display estimated data savings with clear estimate labeling.
- REQ-07 - Refresh filter data without reinstalling the extension.
- REQ-08 - Validate filter updates and preserve the previous valid rules on failure.
- REQ-10 - Persist settings, filter metadata, and summary statistics across restarts.
- NFR-01 - Meet the request-filtering latency target.
- NFR-03 - Meet the defined usability target.

### Could

- REQ-09 - Show filter-update status and user-readable refresh failures on the Dashboard.

### Won't for This Release

- Cloud user accounts or cloud synchronization of settings or browsing data.
- A mobile-native Ad-Vocates application.
- A full user-authored filter-rule editor.
- Guaranteed Firefox, Safari, or other non-Chromium browser support in the first release; compatibility may be investigated, but it is not required for the MVP.
- AI-based classification of advertisements or trackers.

## 8. Inherited Constraints

These constraints are inherited from the team's Week 3 `Constraints_Essay.md` and are assigned stable IDs for traceability.

| ID | Constraint | Design Consequence |
|---|---|---|
| **CON-01** | **Security and privacy:** Ad-Vocates must block trackers and suspicious requests without undermining user privacy, must minimize data collection, and must verify filter sources before applying updates. | Browsing-related data remains local; remote filter retrieval is limited to approved HTTPS sources; downloaded filter data must be validated before activation. |
| **CON-02** | **Ethical transparency:** Protection choices must be understandable to the user, and data-savings values must not be presented as exact measurements when they are estimates. | Dashboard controls must clearly show protection/allowlist state, and estimated data savings must be explicitly labeled as an estimate. |
| **CON-03** | **Website usability:** Blocking can break legitimate website features, so privacy/protection must be balanced with recoverability and user control. | The design must include allowlisting and protection controls so a user can recover from breakage without disabling Ad-Vocates everywhere. |

## 9. Assumptions and Open Questions

### Assumptions

| ID | Assumption | Owner | Review Date |
|---|---|---|---|
| **ASM-01** | The initial implementation will run as a local browser extension and will not require a hosted backend. | Max Marsh | 2026-11-06 |
| **ASM-02** | The first implementation will target Chromium-based browsers before any broader browser-support commitment is made. | Max Marsh| 2026-11-06 |
| **ASM-03** | Normal installations will use approximately 10,000 active filter rules and fewer than approximately 100 allowlisted hostnames for initial performance planning. | Dylan Shackleford | 2026-11-13 |

### Open Questions

| ID | Open Question | Owner | Target Decision Date |
|---|---|---|---|
| **OQ-01** | Which maintained external filter list or combination of lists will be selected after license, compatibility, update frequency, size, and performance review? | Dylan Shackleford | 2026-11-06 |
| **OQ-02** | Does the final filter-list size require IndexedDB for rule storage, or is browser extension storage sufficient for all stored data? | Max Marsh | 2026-11-13 |
| **OQ-03** | Which exact Chromium request-blocking API and rule representation best satisfy the Filter Request design after browser-support investigation? | Dylan Shackleford | 2026-11-13 |

## 10. Source Documents

This requirements baseline consolidates the team's existing repository material rather than introducing a new project scope:

- [Project Description](https://github.com/dylan-shack/Ad-Vocates/blob/main/Project-Description.md)
- [User Stories and Use Cases](https://github.com/dylan-shack/Ad-Vocates/blob/main/User_Stories.md)
- [Constraints Essay](https://github.com/dylan-shack/Ad-Vocates/blob/main/Constraints_Essay.md)
- [D0 High-Level Design](https://github.com/dylan-shack/Ad-Vocates/blob/main/Design_Diagrams/D0_High_Level_Design.md)
- [D1 Detailed Design](https://github.com/dylan-shack/Ad-Vocates/blob/main/Design_Diagrams/D1_Detailed_Design.md)
- [D2 Detailed Design](https://github.com/dylan-shack/Ad-Vocates/blob/main/Design_Diagrams/D2_Detailed_Design.pdf)
- [Week 8 Task List](https://github.com/dylan-shack/Ad-Vocates/blob/main/Tasklist.md)
- [Week 8 Planning Package](https://github.com/dylan-shack/Ad-Vocates/blob/main/Planning/TaskList_Milestones_Timeline_EffortMatrix.pdf)
