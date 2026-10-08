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
