# Ad-Vocates User Stories and Use Cases

**Project:** Ad-Vocates  
**Team Members:** Max Marsh and Dylan Shackleford  
**Course:** Senior Design  
**Assignment:** Assignment 4 - User Stories and Use Cases  

---

## Stakeholder Map

The stakeholders for Ad-Vocates include people who directly use the application, people who help maintain it, and people who may be affected by how the blocker works.

### Primary Stakeholders

- **Privacy-conscious web users:** People who want to reduce advertisements, tracking, and unnecessary third-party requests while browsing.
- **Bandwidth-conscious web users:** People who want to reduce unnecessary network traffic and see how much unwanted content is being prevented from loading.

### Secondary Stakeholders

- **Ad-Vocates maintainers:** Team members responsible for updating filtering rules, fixing bugs, testing changes, and maintaining the application.

### Hidden Stakeholders

- **Users who depend on accessibility or important website features:** Blocking the wrong resource could cause part of a website to stop working correctly.
- **Website operators:** Websites may be affected if legitimate resources are accidentally blocked.

---

# User Stories

## US-01 (Primary)

**As a privacy-conscious web user,**  
**I want to block known advertising and tracking requests while I browse,**  
**so that fewer third parties can track my activity and unwanted content does not have to load.**

---

## US-02 (Primary)

**As a bandwidth-conscious web user,**  
**I want to know how many unwanted requests have been blocked and how much data they would have used,**  
**so that I can better understand the effect Ad-Vocates is having while I browse.**

---

## US-03 (Primary)

**As a web user who trusts a specific website,**  
**I want to allow that website to bypass blocking rules,**  
**so that I can use or support the website without turning off protection everywhere else.**

---

## US-04 (Secondary)

**As an Ad-Vocates maintainer,**  
**I want filtering rules to update from approved sources without requiring users to reinstall the application,**  
**so that the blocker can continue responding to new advertising and tracking domains.**

---

## US-05 (Hidden)

**As a web user who depends on an important website or accessibility feature,**  
**I want a way to recover if filtering causes that feature to stop working,**  
**so that I can continue using the website without disabling Ad-Vocates everywhere else.**

---

# INVEST Self-Check

| Story | Independent | Negotiable | Valuable | Estimable | Small | Testable |
|---|---|---|---|---|---|---|
| US-01 | Yes | Yes | Yes | Yes | Yes | Yes |
| US-02 | Yes | Yes | Yes | Yes | Yes | Yes |
| US-03 | Yes | Yes | Yes | Yes | Yes | Yes |
| US-04 | Yes | Yes | Yes | Yes | Yes | Yes |
| US-05 | Yes | Yes | Yes | Yes | Yes | Yes |

### INVEST Notes

- **Independent:** Each story focuses on a different need and can be worked on separately.
- **Negotiable:** The stories describe what the stakeholder needs without requiring one specific design.
- **Valuable:** Each story gives a clear benefit to a stakeholder.
- **Estimable:** The amount of work for each story can be estimated once the technical design is finalized.
- **Small:** Each story focuses on one main requirement.
- **Testable:** Each story can be tested using an observable result, such as whether a request is blocked, allowed, counted, or handled correctly.

---

# Use Cases

## UC-01: Block a Matched Request

**Expands:** US-01

### Use Case Name

Block a Matched Request

### Primary Actor

Privacy-conscious web user

### Secondary Actors

- Web browser
- Website being visited
- Ad-Vocates filtering rules

### Preconditions

1. Ad-Vocates is installed and enabled.
2. A valid filter list has been loaded.
3. The website being visited is not on the user's allowlist.
4. The active filter list contains a rule that can be matched against a test request.

### Main Success Flow

1. **Actor:** The user opens a website that is not on the allowlist.
2. **System:** Ad-Vocates applies its filtering rules while the website loads.
3. **Actor:** The browser attempts to load a resource that matches an advertising or tracking rule.
4. **System:** Ad-Vocates checks the request against the active filter list and finds a match.
5. **Actor:** The browser waits for the request to either be allowed or blocked.
6. **System:** Ad-Vocates blocks the matched request before the resource finishes loading.
7. **Actor:** The user continues browsing the website.
8. **System:** Ad-Vocates increases the blocked request count by one.

### Alternate Flow: Request Does Not Match a Rule

1. The browser attempts to load a resource.
2. Ad-Vocates checks the request against the active filtering rules.
3. No matching rule is found.
4. Ad-Vocates allows the request to continue.
5. The blocked request count does not change.

### Exception Flow: Invalid Filtering Data

1. Ad-Vocates attempts to load a new set of filtering rules.
2. The new filtering data is invalid or cannot be processed correctly.
3. Ad-Vocates does not activate the invalid filtering data.
4. The most recent valid filtering rules remain active.
5. Ad-Vocates continues running normally.

### Postcondition

The request that matched an active blocking rule has been prevented from loading, and the blocked request total has increased by one.

---

## Acceptance Criteria for UC-01

### AC-01.1: Main Success Flow

**Given** Ad-Vocates is enabled, the current website is not allowlisted, and a requested resource matches an active blocking rule,  
**When** the browser attempts to load that resource,  
**Then** the resource is prevented from completing and the blocked request count increases by exactly one.

### AC-01.2: Exception Flow

**Given** Ad-Vocates already has a valid active filter list and a newly retrieved filter list fails validation,  
**When** Ad-Vocates attempts to activate the new filter list,  
**Then** the invalid list is not activated and the previous valid filter list remains active.

---

# UC-02: Allow a Site When Blocking Causes Problems

**Expands:** US-05

### Use Case Name

Allow a Site When Blocking Causes Problems

### Primary Actor

Web user who depends on an important website or accessibility feature

### Secondary Actors

- Web browser
- Website being visited
- Ad-Vocates filtering system

### Preconditions

1. Ad-Vocates is installed and enabled.
2. The current website is not already on the allowlist.
3. At least one request from the website would normally be blocked by an active rule.
4. The user notices that an important part of the website is not working correctly.

### Main Success Flow

1. **Actor:** The user visits a website and notices that an important part of the site is not working while Ad-Vocates is active.
2. **System:** Ad-Vocates continues using its normal filtering rules on the website.
3. **Actor:** The user chooses to allow the current website.
4. **System:** Ad-Vocates checks the website address and adds the valid hostname to the allowlist.
5. **Actor:** The user reloads the website.
6. **System:** Ad-Vocates recognizes that the website is allowlisted and does not apply its normal blocking rules to the site.
7. **Actor:** The user continues using the website.
8. **System:** Ad-Vocates remains active on websites that are not on the allowlist.

### Alternate Flow: Remove a Site From the Allowlist

1. The user removes a previously allowed website from the allowlist.
2. Ad-Vocates removes the saved exception for that website.
3. The user reloads or revisits the website.
4. Ad-Vocates begins applying its normal filtering rules again.

### Exception Flow: Invalid Website Address

1. The user attempts to add an empty or invalid website address to the allowlist.
2. Ad-Vocates checks the address and determines that it is not valid.
3. Ad-Vocates rejects the request.
4. No new allowlist entry is saved.
5. The existing protection settings stay the same.

### Postcondition

The selected valid website is stored on the allowlist and can load without normal Ad-Vocates filtering. Protection remains active for websites that are not on the allowlist.

---

## Acceptance Criteria for UC-02

### AC-02.1: Main Success Flow

**Given** Ad-Vocates is enabled and a valid website hostname has been added to the allowlist,  
**When** the user reloads that website and it requests a resource that would normally match a blocking rule,  
**Then** the request is allowed on that website while blocking remains enabled on a separate website that is not allowlisted.

### AC-02.2: Exception Flow

**Given** Ad-Vocates is enabled,  
**When** the user attempts to add an empty or invalid hostname to the allowlist,  
**Then** no new allowlist entry is saved and the existing protection settings remain unchanged.
