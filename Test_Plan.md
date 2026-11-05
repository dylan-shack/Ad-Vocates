# **Ad-Vocates Test Plan**

Dylan Shackleford and Max Marsh

# **1\. Test scope**

We will test the Chromium extension in spring 2027\. Dylan will lead testing. Both group members will write the cases for our components.

| Requirements | Component and owner |
| :---- | :---- |
| REQ-01, REQ-02, REQ-04, NFR-01 | Max: Filter Request and Settings. Test which requests get blocked, which sites are allowed and how long each decision takes. |
| REQ-03, REQ-10, NFR-04 | Max: Settings and Local Storage. Test allowlist changes, failed saves and restarts. Dylan tests failed updates. |
| REQ-05, REQ-06, NFR-03 | Dylan: Statistic Recorder. Max: Dashboard. Check the displayed totals and test whether users can find them and change settings. |
| REQ-07, REQ-08, NFR-02 | Dylan: Filter Updater and Filter Validator. Test approved sources, invalid downloads and what data leaves the extension. |

REQ-09, the Dashboard update-status display is deferred since it is optional. General pop-up handling is also deferred until its behavior is defined.

# **2\. Verification and validation strategy**

We will verify the requirements in section 3 by comparing each result with its expected value. For blocking, we know which requests should pass because of the rule list. For storage and updates, we will save a working config, cause a failure, and check which settings and rules the extension uses afterward.

Five users will test NFR-03. We will ask each person to change protection, allow a broken site, and find the statistics without help. The advisor will review whether these controls let users restore one site while protecting on others, whether the savings are clearly marked as estimates, and whether browsing data stays local, which covers CON-01 through CON-03

| Layer | Approx. count | Coverage focus |
| :---- | :---- | :---- |
| Unit | 10 | Rule matching, invalid entries, duplicate events and timing. |
| Integration | 8 | Settings, storage, filter updates and statistics. |
| System | 4 | Browser behavior, network traffic and restarts. |
| Acceptance | 3 | Protection, site recovery and statistics with five users. |

# **3\. Measurable success criteria**

| Metric | Target | How we will measure it | Requirement |
| :---- | :---- | :---- | :---- |
| Blocking | 20 of 20 matching requests blocked. | Compare each decision with the rule list. | REQ-01 |
| Site exception | 10 of 10 matching requests allowed on the listed site. | Run the requests on an allowed site and an unlisted site. | REQ-02 |
| Allowlist edits | Valid additions and removals survive reload. Zero invalid or duplicate entries. | Read the saved list after each edit and reload. | REQ-03 |
| Protection switch | 20 of 20 matching requests allowed while off. Blocking resumes when turned on. | Run the same requests with protection off and on. | REQ-04 |
| Blocked count | One count per unique block. Display updates within 1 second. | Compare the event IDs and times with the displayed count. | REQ-05 |
| Estimated savings | Displayed sum matches the supplied byte estimates within 1 second and is labeled estimated. | Add the known byte values and time the display change. | REQ-06 |
| Rule updates | New valid rules work in the current session. Failed downloads or invalid lists leave the previous rules active. | Request a hostname added by the update. Compare the active list before and after a failure. | REQ-07, REQ-08 |
| Saved data | Settings, allowlist, filter metadata and totals match after 10 of 10 restarts. | Compare saved values after each restart. | REQ-10 |
| Decision time | At least 950 of 1,000 decisions take 50 ms or less with 10,000 rules loaded. | Time each call to evaluateRequest with performance.now(). | NFR-01 |
| Privacy | All sources use HTTPS. Zero invalid lists activated. Zero browsing records sent to unrelated services. | Inspect source settings, active rules and outgoing extension traffic. | NFR-02 |
| Usability | At least 4 of 5 users complete each task unaided in 2 minutes or less. | Time each task separately for each user. | NFR-03 |
| Reliability | Zero crashes in 1,000 requests. Rules survive 10 of 10 failed updates. Settings survive 10 of 10 restarts. | Count finished requests and compare saved values after each trial. | NFR-04 |

# **4\. Test cases**

Start each case with the setup in section 5\.

| ID | Description | Steps | Expected Result | Status |
| :---: | :---- | :---- | :---- | :---: |
| TC-001 | Block listed hosts (REQ-01) | 1\. Open test page with protection on.2\. Request the 20 blocked scripts. | All 20 return BLOCK with a matching rule ID. | Not Run |
| TC-018 | Unlisted host (REQ-01) | 1\. Request a script from a host outside the block list. 2\. Repeat with a second unlisted host | Both return ALLOW. The count stays at 0\. | Not Run |
| TC-027 | Two matching rules (REQ-01, REQ-05) | 1\. Choose a request host whose parent domain is already blocked. Add a rule for the request host itself. 2\. Request one script from that host. | One request is blocked. The displayed count reaches 1 within 1 second. | Not Run |
| TC-002 | Allow one site (REQ-02) | 1\. Add the test page’s hostname to the allowlist. 2\. Load the first 10 blocked scripts on that page. Repeat on a page outside the allowlist. | All 10 load on page.test. All 10 are blocked on other.test. | Not Run |
| TC-003 | Add and remove a hostname (REQ-02, REQ-03) | 1\. Enter the test page’s hostname in mixed case on the allowlist. Reload the extension and load a blocked script on that page. 2\. Remove the entry, reload and request the same script. | One lowercase entry is saved and the first request loads. After removal the entry is gone and the request is blocked. | Not Run |
| TC-022 | Invalid or duplicate hostname (REQ-03) | 1\. Add the test page’s hostname twice, first in lowercase and then in uppercase. 2\. Try a blank entry and a hostname containing a space. Reopen the list. | Only one page.test entry is saved. Both invalid entries are rejected. | Not Run |
| TC-004 | Turn protection off (REQ-04) | 1\. Turn protection off in the Dashboard.2\. Request the 20 blocked scripts. | All 20 load. The count stays at 0\. | Not Run |
| TC-025 | Failed change (REQ-04) | 1\. Set the next settings write to fail.2\. Turn protection off and request from a blocked host | The save reports an error. Protection stays on and the request is blocked. | Not Run |
| TC-005 | Update the Dashboard (REQ-05, REQ-06) | 1\. Open the Dashboard at zero. Send one block event with an estimate of 100 bytes.2\. Time both display changes from the event. | The count reaches 1 and estimated savings reach 100 bytes within 1 second. | Not Run |
| TC-026 | Duplicate event (REQ-05) | 1\. Send one 100-byte block event with ID e1.2\. Send it again with the same ID. Wait for the totals to save. | The saved and displayed totals are 1 block and 100 bytes. | Not Run |
| TC-006 | Add two estimates (REQ-06) | 1\. Send events with different IDs and estimates of 100 and 200 bytes.2\. Wait for each display change before sending the next event. | Savings show 100 then 300 bytes, each within 1 second of its event. The label identifies an estimate. | Not Run |
| TC-007 | Install valid rules (REQ-07) | 1\. Add a previously unblocked host to the approved list and refresh the filters. 2\. Request a script from that host in the same session | The request is blocked without reloading or reinstalling the extension. | Not Run |
| TC-008 | Invalid line in an update (REQ-07, REQ-08, NFR-02) | 1\. Add a previously unblocked hostname and a hostname containing a space on separate lines of the approved list. Refresh the filters. 2\. Remove the invalid line, refresh again and request a script from the valid host | The first refresh rejects the whole list and keeps the old rules. The corrected list blocks the new host in the same session. | Not Run |
| TC-015 | Unapproved redirect (REQ-07, REQ-08, NFR-02, CON-01) | 1\. Redirect the approved download to an unapproved host. Refresh. 2\. Remove the redirect, add a previously unblocked host to the approved list and refresh. Request a script from that host | The redirect is rejected and the old rules stay active. The approved update blocks newads.test in the same session. | Not Run |
| TC-032 | Repeated download failures (REQ-08, NFR-04) | 1\. Save a copy of the active rules. Make five downloads time out after 30 seconds and five return an empty list. 2\. After each failure compare the rules and request a script from a blocked host | The rules match the saved copy and the request is blocked in all 10 trials. | Not Run |
| TC-010 | Restart with saved data (REQ-10, NFR-04) | 1\. Save the restart values in section 5\. Reload the extension five times, then close and reopen Chrome five times.2\. Compare the saved values after every restart. | All values match on 10 of 10 restarts. | Not Run |
| TC-034 | Restart after a change (REQ-10) | 1\. Start with the restart values. Enable protection and remove page.test. Wait for the save and restart Chrome.2\. Check settings, filter metadata and both totals. | Protection is on, the list is empty, and the original metadata and totals are retained. | Not Run |
| TC-035 | Restart after a failed save (REQ-03, REQ-10) | 1\. Save the restart values. Make the next settings write fail.2\. Remove page.test, restart Chrome and read the saved values. | The save reports an error. After restart, page.test and all other saved values match the original. | Not Run |
| TC-011 | Time mixed requests (NFR-01) | 1\. Load the 10,000-rule list. Run the mixed requests from section 5\.2\. Measure each decision from call to return. | At least 950 of 1,000 take 50 ms or less. Exactly 500 are blocked. | Not Run |
| TC-036 | Time allowlist checks (NFR-01) | 1\. Load the 10,000-rule list and allow page.test.2\. Time the same 1,000 mixed requests from page.test. | All 1,000 are allowed. At least 950 take 50 ms or less. | Not Run |
| TC-014 | Stress the filter (NFR-01, NFR-04) | 1\. Load the 10,000-rule list. Request clean1.test/a.js 999 times with different IDs.2\. Use “bad url” for the last call. Time all 1,000 sequential decisions. | All calls finish without a crash. The last returns INVALID\_URL. At least 950 take 50 ms or less. | Not Run |
| TC-012 | Inspect network traffic (NFR-02) | 1\. Check each configured source URL. Record outgoing extension traffic while loading a script on the test page and refreshing the filters. Note the page and script URLs. 2\. Inspect destinations, headers and bodies for either URL or other browsing history. | Every configured source uses HTTPS. Zero browsing records go to unrelated services. | Not Run |
| TC-013 | Change protection (REQ-04, NFR-03) | 1\. Ask each of five users to turn protection off and back on. 2\. Time the task without hints. Then request a script from a blocked host | At least 4 of 5 finish in 120 seconds or less. The request is blocked after protection is turned back on. | Not Run |
| TC-017 | Restore a broken site (REQ-02, REQ-03, NFR-03, CON-03) | 1\. Ask each user to allow page.test, reload it and click its button. Test the same script on sub.page.test.2\. Have the user remove the exception and reload. Time the user’s actions without hints. | The button works only on the allowed host and stops working after removal. At least 4 of 5 finish unaided in 120 seconds or less. | Not Run |
| TC-016 | Find the totals (REQ-06, NFR-03, CON-02) | 1\. Set 20 blocks and 300 estimated bytes. Send a new block with no byte estimate.2\. Ask each user to find the count and savings. Time each user without hints. | The Dashboard shows 21 blocks and 300 bytes labeled estimated. At least 4 of 5 find both values in 120 seconds or less. | Not Run |

# **5\. Test environment**

| Environment | Setup |
| :---- | :---- |
| Development | Planned test computer: Latest versions of Windows 11 and Chrome, confirm versions at IM01. Use JavaScript tests to simulate a failed write, timeout or bad download. |
| Staging | Load the unpacked extension in a clean Chrome profile on the same computer. Use a local web server for the test pages and an approved HTTPS test source for filter updates. Use a normal connection except when a case calls for a failure. |
| Pilot | Use the same build and computer for all five users. Reset the profile before each task. Start the timer after reading the task and stop when the user finishes. |

Before each case, turn protection on, clear the allowlist and set both totals to zero. Load 20 hostnames into the block

list, one per line. Use a separate test page that requests one script from each blocked host. Give each request a

different ID. Keep Dashboard open when timing a display change.

For the broken-site test, serve the same page on a host and one of its subdomains. Give the page a button that displays

a message using a script from a blocked host. For timing tests, load 10,000 hostname rules. Alternate between 500

requests to listed hosts and 500 to unlisted hosts.

For restart tests, save protection off, one test page hostname on the allowlist, 20 blocked requests and 500 estimated

bytes. Record the active filter metadata before restarting.

# **6\. Pass and fail criteria**

Mark a case Pass if every expected result is met. Mark it Fail if any result or threshold is missed. Use Blocked when something prevents the test from running and Not Run before the first attempt.

The plan passes when all 25 cases pass and the advisor review is complete. Each of the three user tasks must meet the 4-of-5 target on its own. A failed, blocked or unrun case keeps the plan from passing.

# **7\. Test schedule**

| Milestone | Test activity | Owner | Evidence |
| :---- | :---- | :---- | :---- |
| IM01–IM02, Jan. 14–20 | Confirm browser support and the filter format. Run unit tests for invalid filters, saved records and failed writes. | Dylan: filters. Max: storage. | Browser version, rule lists and saved values. |
| IM03, Feb. 3 | Test settings and updates together. Try an invalid download and a failed settings change. | Max: settings. Dylan: updates. | Settings and rules before and after each failure. |
| IM04–IM05, Feb. 17–Mar. 3 | Test blocking and counting during T6/T7. Check Dashboard totals and timing during T8. | Max: filter and Dashboard. Dylan: counts. | Request decisions, totals and timings. |
| IM06, Mar. 24 | During T9, run the system tests for restarts, repeated failures, privacy and speed. | Dylan and Max | Restart results, network capture and timings. |
| IM07–IM08, Apr. 14–23 | Run the five-user tests before IM07. Fix and retest failures before the IM08 advisor demo. | Max: user tasks. Dylan: results. | Task times, retest results and advisor review. |

# **8\. Definition of done**

## **Feature checklist**

\[ \] The component tests pass, including the edge or failure case.

\[ \] The other team member has reviewed the change and its documentation.

\[ \] Any defect that prevents the feature from meeting its requirement is fixed and retested.

## **Project checklist**

\[ \] All 25 cases pass and the results are recorded.

\[ \] The installation steps, test plan and traceability matrix are up to date.

\[ \] Both team members have reviewed the open issues and signed off. Advisor review is recorded.