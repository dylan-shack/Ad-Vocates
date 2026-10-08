# D1 Detailed System Design

## 1. Header, Scope, and Conventions

**Project Title:** Ad-Vocates

**Team Members:** Dylan Shackleford, Max Marsh

**Goal:** Create a reliable and efficient tool that improves browsing performance while giving users greater privacy, security, and control over their web experience.

This D1 document provides a more detailed design for the **Filter Request**, **Local Storage**, and **Settings** components from D0. The **Dashboard**, **Statistic Recorder**, **Filter Updater**, and **Filter Validator** components will be detailed further in D2.

### Figure Conventions

The D1 figures use the following conventions:

- Rectangular boxes represent system components or stored data entities.
- Lines between entities represent relationships between stored data.
- Arrows between system components represent the direction of communication or data flow.
- Primary keys are marked with **PK**.
- Foreign keys are marked with **FK**.
- Relationship cardinality is shown using **1:1**, **1:M**, or **M:N** notation.
- Labels on arrows describe the type of data being passed between components.
- Components already defined in D0 keep the same names in D1 so the two designs can be compared directly.
## 2. Data Model, the D1 Diagram

The main data model for Ad-Vocates is contained within the **Local Storage** component from D0. Since Ad-Vocates runs locally as a browser extension, the project does not require a traditional server-side relational database. The stored information is small, belongs to one local installation of the extension, and does not need complex joins between tables.

For this reason, Ad-Vocates will use a **local object-based storage model** rather than a relational database. The model can be implemented using browser-local storage such as IndexedDB or another browser storage API. This keeps browsing-related information on the user's device and follows the D0 decision that filtering and application data should remain local.

The core stored records are shown below.

### Data Entities

#### ApplicationSettings

Stores the main protection setting for the local Ad-Vocates installation.

| Attribute | Type | Description |
|---|---|---|
| `settingsId` | string, PK | Unique identifier for the settings record |
| `enabled` | boolean | Indicates whether Ad-Vocates protection is enabled |

There will normally be one `ApplicationSettings` record for an installation.

---

#### AllowlistEntry

Stores websites that the user has chosen to allow without normal filtering.

| Attribute | Type | Description |
|---|---|---|
| `hostname` | string, PK | Normalized hostname of the allowed website |
| `settingsId` | string, FK | Connects the entry to the application settings |

One `ApplicationSettings` record can have zero or many `AllowlistEntry` records.

This supports **US-03** and **US-05**, which require users to allow trusted sites or recover when blocking prevents an important website feature from working.

---

#### FilterRule

Stores the active filtering rules used by the Filter Request component.

| Attribute | Type | Description |
|---|---|---|
| `ruleId` | string, PK | Unique identifier for the filter rule |
| `ruleText` | string | The filter rule or pattern that will be evaluated |
| `settingsId` | string, FK | Associates the rule with the active application settings |

One `ApplicationSettings` record can have zero or many `FilterRule` records.

This supports **US-01** and **US-04**, because the rules are used to identify requests that should be blocked and can be replaced or updated as new filter data becomes available.

---

#### StatisticsSummary

Stores the current blocking totals used by the dashboard.

| Attribute | Type | Description |
|---|---|---|
| `statsId` | string, PK | Unique identifier for the statistics record |
| `blockedTotal` | integer | Total number of blocked requests |
| `estimatedBytes` | integer | Estimated number of bytes prevented from loading |

The application will normally maintain one current `StatisticsSummary` record rather than storing the user's complete browsing history.

This supports **US-02** and follows the D0 decision to store totals instead of individual browsing-history records.

---

#### ErrorLog

Stores errors that need to be recorded when filtering or local data operations fail.

| Attribute | Type | Description |
|---|---|---|
| `errorId` | string, PK | Unique identifier for the error |
| `errorMsg` | string | Description of the error |
| `loggedAt` | string | Time the error was recorded in ISO 8601 format |

Multiple `ErrorLog` records may exist. These records are intended for application errors and debugging and do not contain a user's browsing history.

---

### Relationships and Cardinality

The main relationships in the D1 data model are:

- `ApplicationSettings (1)` to `AllowlistEntry (0..*)`
- `ApplicationSettings (1)` to `FilterRule (0..*)`
- `StatisticsSummary` is maintained as one local summary record.
- `ErrorLog` may contain zero or many independent error records.

The allowlist and filter rules are represented as separate records instead of being stored as one large value because individual entries may need to be added, removed, checked, or updated without replacing unrelated data.

### Structural Decisions

| Decision | Reason |
|---|---|
| `AllowlistEntry` is an entity instead of an attribute containing one large list | Each hostname needs to be checked, added, or removed independently. This directly supports US-03 and US-05. |
| `FilterRule` is an entity instead of one large filter string | Individual rules need to be loaded, validated, updated, and checked during filtering. This supports US-01 and US-04. |
| `ApplicationSettings` has a one-to-many relationship with `AllowlistEntry` | One local installation has one active set of settings but may contain many allowed websites. |
| `ApplicationSettings` has a one-to-many relationship with `FilterRule` | One active configuration may use many filtering rules at the same time. |
| Statistics are stored as a summary instead of individual browsing records | The dashboard needs blocked totals and estimated bytes, but storing full browsing history would collect unnecessary user information. |
| A local object store is used instead of a relational database | The application runs locally, has a small data model, does not require complex joins, and should avoid requiring a remote database or server. |

### Indexing Decisions

`hostname` is the primary key for `AllowlistEntry` so that the system can quickly determine whether the current website is allowlisted before filtering requests.

`ruleId` is the primary key for `FilterRule` so that duplicate rules can be identified and individual rules can be replaced or removed during filter updates. The actual request-matching process will use the loaded rule data rather than performing a database lookup for every browser request.

`statsId` does not require an additional secondary index because only one current statistics summary is expected.


If error records are retained, `loggedAt` may be indexed so recent errors can be retrieved in time order during debugging.


## 3. Core Algorithms

The two main algorithms for Ad-Vocates are request filtering and allowlist checking. These are important because if either one is too slow or does not work correctly, the blocker could miss unwanted requests or block something that should be allowed.

### Algorithm 1: Request Filtering and Rule Matching

#### What it does

The request filtering algorithm decides whether a browser request should be blocked or allowed.

When the browser tries to load a resource, Ad-Vocates checks the request against the active filtering rules. If a matching blocking rule is found, the request is blocked. If no rule matches, the request is allowed to continue.

This algorithm supports **US-01**, which requires known advertising and tracking requests to be blocked.

#### Inputs

| Input | Type | Description |
|---|---|---|
| `requestUrl` | `string` | Full URL of the requested resource |
| `pageHost` | `string` | Hostname of the page making the request |
| `resourceType` | `string` | Type of resource being requested, such as script, image, or frame |
| `enabled` | `boolean` | Whether Ad-Vocates protection is currently enabled |
| `activeRules` | `FilterRule[]` | Collection of active filtering rules |

Each `FilterRule` contains:

`ruleId: string`  
`ruleText: string`

#### Output

The algorithm returns:

`decision: "ALLOW" | "BLOCK"`  
`matchedRuleId: string | null`

If the request is blocked, `matchedRuleId` identifies the rule that caused the block. If the request is allowed, `matchedRuleId` is `null`.

#### Expected Complexity

The active filter rules will be loaded into memory so that Local Storage does not need to be accessed for every request.

For the first version of the project, the rules can be grouped by domain or another useful identifier so Ad-Vocates only checks rules that could apply to the current request.

The expected lookup is approximately `O(1 + k)`, where `k` is the number of candidate rules that need to be checked for the requested domain.

For example, if Ad-Vocates contains around **10,000 active rules**, only a small subset of those rules should normally be checked for one request.

If the rule set increased by 100 times to around **1,000,000 rules**, checking every rule for every request would not be practical. The rules would need to stay indexed or grouped so the number of rules checked for one request stays relatively small.

This matters because the filtering algorithm may run many times while a single webpage is loading.

#### Why This Approach Was Chosen

The team chose to load and organize filter rules in memory instead of reading every rule from Local Storage for every request.

A simple linear scan through every active rule would be easier to implement, but it would take approximately `O(n)` time for every request, where `n` is the total number of filter rules. This would become slower as the filter list grows.

Organizing the rules before request processing takes some extra setup, but it allows the Filter Request component to make faster decisions during normal browsing.

#### Edge Cases

- If no filtering rules are loaded, the request is allowed.
- If protection is disabled, the request is allowed without checking the rules.
- Duplicate rules should not cause the blocked request counter to increase more than once for the same request.
- If multiple rules match, the request is still only blocked once.
- If a rule cannot be processed, it should be skipped instead of stopping the entire filtering process.
- If the request URL cannot be parsed, Ad-Vocates should not crash.
- If no rule matches, the request is allowed.

---

### Algorithm 2: Allowlist Checking

#### What it does

Before normal filtering is applied, Ad-Vocates checks whether the current website has been added to the allowlist.

If the website is allowlisted, normal blocking rules are skipped for that site. If the website is not allowlisted, request filtering continues normally.

This algorithm supports **US-03** and **US-05**, because users need a way to trust a website or recover if blocking causes an important website feature to stop working.

#### Inputs

| Input | Type | Description |
|---|---|---|
| `pageHost` | `string` | Hostname of the website currently being visited |
| `allowlist` | `Set<string>` | Set of normalized hostnames that the user has allowed |

#### Output

The algorithm returns:

`isAllowlisted: boolean`

If the value is `true`, normal filtering should be skipped for the website.

If the value is `false`, Ad-Vocates continues with the normal filtering process.

#### Expected Complexity

The allowlist will be stored in a set-like structure so checking whether a hostname exists has an expected complexity of `O(1)`.

For a normal installation, the allowlist is expected to contain fewer than around **100 websites**.

Even if the allowlist grew by 100 times to around **10,000 websites**, an average `O(1)` lookup should still have very little effect on browsing performance.

#### Why This Approach Was Chosen

A set or hash-based lookup was chosen because the system only needs to determine whether a hostname is currently allowlisted.

An alternative would be storing the allowlist as an array and checking each hostname one at a time. That would require an `O(n)` search and would become less efficient as the allowlist grows.

A set also helps prevent duplicate hostname entries.

#### Edge Cases

- If the allowlist is empty, the hostname is treated as not allowlisted.
- Duplicate hostnames should not be stored.
- Hostnames should be converted to lowercase before comparison.
- Hostnames should be normalized before lookup.
- Subdomains should be treated separately unless the team later decides to support parent-domain matching.
- Invalid or empty hostnames should not be added to the allowlist.
- If a hostname is removed from the allowlist, normal filtering should resume for that site.
