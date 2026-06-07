# HUNTD-52 — "Year" Field on Experience Page Accepts Special Characters and Silently Saves Them as Year 2001

**Severity:** Major  
**Priority:** High

---

## Environment

| | |
|---|---|
| Browser | Microsoft Edge 148.0.3967.70 (64-bit), Google Chrome 148.0.7778.168 (64-bit) |
| OS | Windows 10 Pro |

---

## Preconditions

User is completing Candidate registration flow.

---

## Steps to Reproduce

1. Navigate to [Experience](https://huntd.tech/profile/candidate/experience)
2. Click `[Add Manually]`
3. Enter `` ` `` in the Start Date "Year" field
4. Select any month in the Start Date "Month" dropdown
5. Enter `` ` `` in the End Date "Year" field
6. Select any month in the End Date "Month" dropdown
7. Fill remaining required fields with valid data
8. Click `[Save]`
9. Observe the saved experience entry in Experience Preview

---

## Expected Result

Special character input is not accepted in the "Year" field. A validation message is displayed.

---

## Actual Result

- Special characters are accepted without validation
- Payload sent: `startDate: "\`-5"` and `endDate: "\`-5"`
- Server silently converts special characters to year `2001`
- Experience entry displays `May 2001 - May 2001 • 1 month`

---

## Additional Notes

All of the following special characters were tested and accepted without validation, each resulting in a silent conversion to year `2001`:

`` ` `` `~` `•` `'` `!` `@` `%` `^` `&` `*` `(` `)` `_` `$`