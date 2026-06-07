# HUNTD-58 — Profile Reactivation After Edit Creates Duplicate Chats and Contact History by Treating Updated Profiles as New Entities

**Severity:** Critical  
**Priority:** High

---

## Environment

| | |
|---|---|
| Browser | Microsoft Edge 148.0.3967.70 (64-bit), Google Chrome 148.0.7778.168 (64-bit) |
| OS | Windows 10 Pro |

---

## Preconditions

- Recruiter and Candidate accounts exist
- Recruiter has previously contacted Candidate via chat
- Candidate or Recruiter edits their profile and reactivates it

---

## Steps to Reproduce

### Scenario 1 — Candidate Edits Profile

1. Recruiter contacts Candidate via Candidates List — chat is created, Contacted Candidates counter shows +1
2. Candidate edits profile → status changes to Draft/In Review → profile is reactivated
3. Recruiter opens Candidates List
4. Observe — previously contacted Candidate reappears with `[Start Chat]` button
5. Recruiter clicks `[Start Chat]` — new chat is created
6. Observe — Contacted Candidates counter increments +1 again for the same Candidate

### Scenario 2 — Recruiter Edits Profile

1. Recruiter contacts Candidate — chat is created
2. Recruiter edits profile → profile is reactivated → new profile ID is generated
3. Recruiter contacts the same Candidate again
4. Observe — a second chat is created for the same Recruiter-Candidate pair
5. Candidate opens Chats — sees 2 separate chats with the same Recruiter

---

## Expected Result

Profile edits preserve existing chat relationships and contact history. Updated profiles are treated as the same entity, not new contacts.

---

## Actual Result

- Previously contacted Candidate reappears as uncontacted after profile edit
- Duplicate chats created for the same Recruiter-Candidate pair
- Contacted Candidates progress bar inflated with duplicate contacts
- Candidate sees multiple versions of the same Recruiter in Chats
- Profile IDs observed changing across edit cycles:
  - `candidateProfileId: 13173` → `13176` → `13179`
  - `recruiterProfileId: 8023` → `8130`

---

## Root Cause

Editing and reactivating a profile generates a new profile ID. The system subsequently treats the updated profile as a new contact entity, breaking all existing recruiter-candidate relationships tied to the previous profile ID. New profile ID generation on edit may be intended architecture — the defect is in broken relationship continuity across profile versions, not the ID generation itself.

Issue is triggered by any profile edit, including trivial changes such as whitespace or punctuation.

---

## Additional Notes

Verified after clearing cookies and site data, re-login, and page reload — ruling out client-side caching as the root cause.