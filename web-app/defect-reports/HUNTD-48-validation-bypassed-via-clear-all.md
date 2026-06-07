# HUNTD-48 — "Desired Roles" Required Field Validation Bypassed After Clearing All Selected Values Using [X] Button

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

User is completing Candidate registration flow.

---

## Steps to Reproduce

1. Navigate to [Role](https://huntd.tech/profile/candidate)
2. Fill all required fields including the "Desired Roles" dropdown
3. Click the clear all `[X]` button on the "Desired Roles" dropdown
4. Click `[Save And Continue]`
5. Observe — form submits despite the empty "Desired Roles" field

---

## Expected Result

Validation message "Select at least 1 role" is displayed and form submission is prevented.

---

## Actual Result

Form submits successfully with an empty "Desired Roles" field.

---

## Root Cause

Confirmed via DevTools Network tab — clicking `[Save And Continue]` after clearing via the `[X]` button sends `specializationsIds: []` (empty array) and `specializationId: null`. The server accepts the empty payload without returning a validation error.

The validation works correctly when the "Desired Roles" field is left empty from the start — the "Select at least 1 role" message is displayed and submission is blocked. The bypass occurs specifically when values are first selected and then cleared using the `[X]` button, which resets the field without re-triggering client-side validation.