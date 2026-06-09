# HUNTD-80 — Recruiter Company Details Mutation Discards All Field Values When One Field Exceeds the 255-Character Limit

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

User is logged in as Recruiter.

---

## Steps to Reproduce

1. Navigate to [Company Details](https://huntd.tech/profile/recruiter/company-info)
2. Enter a valid value in the "Company" field
3. Enter 256+ characters in the "My Role" field
4. Click `[Save Changes]`
5. Observe — error message displayed
6. Observe — `[Save Changes]` button disappears
7. Observe — fields still display the entered values
8. Refresh the page
9. Observe — both "My Role" and "Company" fields are empty

---

## Expected Result

Input exceeding 255 characters is rejected with a validation message. Valid field values are preserved despite the invalid field error. `[Save Changes]` button remains visible indicating the form was not saved. No profile data is lost.

---

## Actual Result

- Error message displayed
- `[Save Changes]` button disappears, suggesting a successful save
- Server rejects the entire mutation — all field values lost, including valid ones
- After page refresh both fields are empty
- Recruiter profile displays "Not filled" for required fields
- Recruiter profile permanently stuck in Draft status

---

## Root Cause

The server enforces a `character varying(255)` constraint at the database level. When any field in the mutation exceeds the limit, the entire `UPDATE` transaction is aborted — including valid values for other fields. The UI dismisses the `[Save Changes]` button on error, misleading the user into thinking the save succeeded.

```json
{
  "errors": [{
    "message": "value too long for type character varying(255)",
    "locations": [{"line": 2, "column": 3}],
    "path": ["updateRecruiterProfile"],
    "extensions": {
      "code": "INTERNAL_SERVER_ERROR",
      "exception": {
        "name": "SequelizeDatabaseError",
        "parent": {
          "code": "22001",
          "routine": "varchar",
          "sql": "UPDATE \"recruiter_profiles\" SET \"position\"=$1,\"company_name\"=$2,\"updated_at\"=$3 WHERE \"id\" = $4",
          "parameters": [
            "TESTNAME...256chars",
            "VS",
            "2026-06-05 19:42:05.367 +00:00",
            8222
          ]
        }
      }
    }
  }],
  "data": null
}
```

---

## Additional Notes

- `userUnreadMessagesCount` query fires on reload, confirming page was refreshed and data loss is not a caching artifact
- Data loss confirmed — valid `companyName: "VS"` discarded along with the invalid field value
- Related to [HUNTD-79](./HUNTD-79-role-company-name-fields-silently-discard-input-exceeding-255-chars.md) — same `varchar(255)` silent failure pattern, but with a more severe consequence: valid field values in the same mutation are also lost