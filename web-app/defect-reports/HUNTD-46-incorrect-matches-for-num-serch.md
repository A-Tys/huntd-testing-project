# HUNTD-46 — "Job Experience" Dropdown Returns Incorrect Matches for Numeric Search Input

**Severity:** Minor  
**Priority:** Low

---

## Environment

| | |
|---|---|
| Browser | Microsoft Edge 148.0.3967.70 (64-bit), Google Chrome 148.0.7778.168 (64-bit) |
| OS | Windows 10 Pro |

---

## Affected Areas

Bug persists across all instances of the "Job Experience" dropdown component:
- Candidate registration — Job Expectations page
- Candidate Edit Profile page
- Candidates List page — Experience filter
- Recruiter registration — Perfect Candidate page

---

## Preconditions

User is completing Candidate registration flow.

---

## Steps to Reproduce

1. Navigate to [Job Expectations](https://huntd.tech/profile/candidate/job-expectations)
2. Click on the "Job Experience" dropdown
3. Type `4` in the search field and observe returned results
4. Clear the field, type `6` and observe returned results
5. Clear the field, type `7+` and observe returned results

---

## Expected Result

| Input | Expected Match |
|---|---|
| `4` | `3-5 years` |
| `6` | `5+ years` |
| `7+` | `5+ years` |

---

## Actual Result

| Input | Actual Match |
|---|---|
| `4` | `5+ years` ❌ |
| `6` | No options ❌ |
| `7+` | No options ❌ |

---

## Additional Notes

Search is client-side — no network requests are fired when typing. The dropdown filter logic does not correctly map numeric input to the range-based option labels (`3-5 years`, `5+ years`), resulting in incorrect matches and missing results for valid numeric inputs.