# Mobile Application — Test Runs Summary

**Project:** Huntd Mobile Project  
**Tester:** Anastasiya Tys  
**Platform:** Android (Motorola Edge 30 Fusion, Android 14)  
**Total Test Cases:** 273  
**Overall Result:** 96% passed

---

## Overall Results

| Status | Count | Share |
|---|:---:|:---:|
| ✅ Passed | 262 | 96% |
| ❌ Failed | 8 | 3% |
| ⛔ Blocked | 2 | 1% |
| ⏭️ Skipped | 1 | 0% |
| 🔵 Untested | 0 | 0% |

---

## Test Runs by Module

| # | Test Run | Date | Pass Rate |
|---|---|---|:---:|
| 1 | Sign Up Test Run | June 03, 2026 | 93% |
| 2 | Sign In Test Run | June 04, 2026 | 100% |
| 3 | Chats Test Run | June 05, 2026 | 95% |
| 4 | Profile Test Run | June 05, 2026 | 99% |

---

## Scope Notes

The mobile application is an MVP focused on IAM, chat features, and profile settings. The following modules are not implemented in the mobile app and were out of scope:

- Candidates List
- Web3 Companies & Jobs
- Footer

Recruiter and Candidate registration flows are simplified in the mobile app — both flows are covered within the Sign Up test run. Candidate registration flow was partially blocked at the Expectations stage due to a Critical bug in the location search field (`FabricViewStateManager: setState called without a StateWrapper`).

---

## About This Report

Test execution was carried out on a physical Android device using ADB via Windows PowerShell and scrcpy for screen mirroring. Grey-box investigation was performed via ADB logcat to confirm root causes for failed cases before filing bug reports in Jira (project: HMP).

All 273 test cases were executed with 0 untested remaining.
