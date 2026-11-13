# **Ad-Vocates Ethics Statement**

Team Members: Dylan Shackleford, Max Marsh

## **Privacy summary**

| Data item | Source | Purpose | Consent | Storage | Access | Retention |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| requestId, requestUrl, pageHost, resourceType | Browser request | Check requests against rules and the allowlist; estimate savings | Browser permission at installation | Memory during request | Filter Request | Request processing only |
| decision, matchedRuleId, errorCode | Filter Request | Return filtering result | Browser permission | Temporary response | Browser, Filter Request | Until response ends |
| eventId, category, estimatedBytes | Blocked request | Count blocked content and estimate savings | Browser permission | Pending memory; recent IDs for duplicates | Statistic Recorder | Pending events removed after saving; recent IDs capped at 10,000 |
| statsId, blockedTotal, total estimatedBytes, unknownEstimateTotal, incomplete | Statistic Recorder | Display blocking totals and incomplete estimates | Browser permission | Local Storage | Statistic Recorder, Dashboard | Until extension data is cleared |
| enabled, revision | Dashboard/Settings | Save protection choice; reject outdated saves | User changes setting | Local Storage | Settings, Dashboard, Filter Request | Until changed or cleared |
| Allowlist hostnames | User | Bypass blocking for chosen sites | User adds/removes each site | Local Storage | Settings, Filter Request, Dashboard | Until removed or cleared |
| Filter rule IDs and hostnames | Approved filter source | Identify blocked hosts | Filter updates described in extension settings | Local Storage; active memory | Filter Updater, Validator, Settings, Filter Request | Until replaced or cleared |
| sourceId, HTTPS source URL | Approved source configuration | Restrict filter downloads | Filter updates described in extension settings | Packaged configuration | Filter Updater, Validator | Until configuration changes |
| lastSuccessAt, nextAttemptAt, failureCount, lastErrorCode | Filter Updater | Track updates and retries | Browser permission | Local Storage | Filter Updater, Dashboard | Until reset or cleared |
| component, errorCode, fixed errorMsg | Extension errors | Identify failures without recording URLs | Browser permission | Local Storage | Extension components, Dashboard | Maximum 100 records; remove after 7 days |
| Message version, requestId, method, payload, status and replies | Internal components | Communicate settings, statistics and errors | Extension operation/user action | Temporary message | Authorized extension components | Until message completes |
| Downloaded filter text and candidate rules | Approved HTTPS source | Validate new rules | Filter updates described in extension settings | Temporary memory | Filter Updater, Validator | Discard after validation; valid rules saved |

At the end of the project, the team will not retain users' browsing data because none is sent to the team; the app functions through local storage.

## **Security summary**

The project does not use any accounts/passwords/auth or public backend API, since it operates as a local extension. authorizeMessage(sender, method) checks that the internal requests come from approved dashboard pages and use valid methods, and unknown sender/methods get rejected.

Filter rules are downloaded only from approved HTTPS sources, and downloads have a 30-second timeout and 10 MB limit with unapproved sources being rejected. No secrets or API keys are part of the planned architecture, so there is no secret storage location or token lifetime. Rate limiting on public endpoints is not applicable, the filter updates are one at a time.

| Self-audit question | Answer |
| :---- | :---- |
| Default credentials? | None; no login system. |
| Unauthenticated endpoints? | No public endpoints; internal messages check sender and method. |
| Secrets in source control? | No application secrets are used. Git-history search for passwords/keys/etc confirmed no secrets exist in the repository. |
| Secure downloads? | Approved HTTPS sources, size/timeout checks, redirect checks, and filter validation. |
| Unnecessary logs? | URLs excluded; maximum 100 records, removed after 7 days. |
| Failed settings or updates? | Preserve previous settings and active filters. |

## **Licensing summary**

The project is in the design phase, so D2 has not finalized the production filter-list source or its license. No copyleft dependencies have been used, so there are currently no copyleft obligations. 

| Dependency, dataset, or asset | Version | License (SPDX) | Type | Obligation / decision |
| :---- | :---- | :---- | :---- | :---- |
| Browser extension APIs | Not selected | N/A | Platform API | Follow the browser's API and distribution terms. |
| Production filter list | Not selected | Pending | Pending | Verify usage and redistribution terms before using. |
| Third-party libraries | None currently | N/A | N/A | Recheck dependencies when implementation begins. |
| Icons, images, fonts, or other bundled assets | None currently | N/A | N/A | Record author, license, and notices if added. |
| Ad-Vocates project | Design phase | Unlicensed | N/A | Currently unlicensed as of design phase \- considering MIT standard license |

## **Attribution list**

The extension uses browser platform functionality, but target browser API and production filter list have not been finalized. No third-party library, adapted tutorial code, dataset, icon, image, or bundled font have been used or incorporated in the design yet. Source URLs, authors, licenses, and restrictions will be recorded here and in the README or NOTICE file when these items are selected. The filter list will be obtained from an approved HTTPS source and validated before use.

## **AI-use disclosure**

Generative AI tools may be used for drafting and code assistance but not for the design decisions or written analysis that are assessed in this course.

| Tool | Used by | Task | Column | How verified |
| :---- | :---- | :---- | :---- | :---- |
| ChatGPT, Claude | Dylan Shackleford | Creating rough drafts/outlines for assignments based on my own ideas, architectures, desired structure and plans | Generally OK | Carefully reviewed drafts, cross-referencing with other documents and re-writing and detailing documents, using AI output as a framework and not a product |
| ChatGPT, Claude | Max Marsh | Creating rough drafts/outlines for assignments based on my own ideas, architectures, desired structure and plans | Generally OK | Carefully reviewed drafts, cross-referencing with other documents and re-writing and detailing documents, using AI output as a framework and not a product |

\_\_***Dylan Shackleford***\_\_  Date: \_\_***11/12/2026***\_\_ — I confirm that the architecture, data model, priorities, and design rationale were our team's own.

\_\_***Max Marsh*** \_\_ Date: \_\_***11/12/2026***\_\_ — I confirm that the architecture, data model, priorities, and design rationale were our team's own.