# HUNTD-57 — Candidate Profiles Returned to "In Review" Status After Edit Remain Displayed in Candidates List

**Severity:** Major  
**Priority:** High

---

## Environment

| | |
|---|---|
| Browser | Microsoft Edge 148.0.3967.70 (64-bit) |
| OS | Windows 10 Pro |

---

## Preconditions

1. Candidate profile was previously Active and visible in Candidates List
2. Candidate edits their profile, causing status to change to "In Review"
3. User is logged in as a Recruiter

---

## Steps to Reproduce

1. Locate a candidate with a previously approved and subsequently edited profile in "In Review" status
2. Search for that candidate on [Candidates](https://huntd.tech/candidates)
3. Open the candidate profile
4. Attempt the contact flow

---

## Expected Result

One of the following behaviors should occur consistently:

- Candidate profile is removed from Candidates List after status changes to "In Review"
- **OR** contact functionality remains fully supported

---

## Actual Result

Profile remains accessible from recruiter-facing flows, but contact becomes unavailable:
- Connection request dialog submits the request and mutation executes
- Backend returns a GraphQL error
- Request creation fails silently

The issue also affects profiles with "Draft" status of a previously approved profile — these remain displayed in Candidates List and recruiter can open them, but contact functionality is unavailable.

---

## Evidence

![Profile displayed with Draft status](../evidence-screenshots/Draft%20status.png)

---

## Additional Notes

- Verified after clearing cookies and site data, re-login, and page reload — profile still displayed, ruling out caching as the cause
- Newly created Candidate profiles with "In Review" status are **not** displayed in Candidates List
- Candidate profiles with "Inactive" status are **not** displayed in Candidates List
- The bug is specific to profiles that transition from Active → edited → "In Review" or "Draft"