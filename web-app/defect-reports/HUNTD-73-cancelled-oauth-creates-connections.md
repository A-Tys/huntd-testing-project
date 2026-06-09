# HUNTD-73 — Cancelled OAuth Authorization Attempts Create Persistent Social Profile Connections and Incorrect [Disconnect] Button States

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

- User originally registered via OAuth provider (e.g. Google)
- Other OAuth providers are not connected

---

## Steps to Reproduce

1. Log in using existing OAuth account (e.g. Google)
2. Navigate to [Social Profiles](https://huntd.tech/settings/social-profiles) — observe remaining OAuth accounts display `[Connect]`
3. Log out and initiate authentication via another provider (e.g. LinkedIn or GitHub)
4. Close the authorization window without granting access or completing authentication
5. Log in using the original OAuth account
6. Navigate to [Social Profiles](https://huntd.tech/settings/social-profiles) — observe remaining OAuth accounts now display `[Disconnect]`

---

## Expected Result

- Incomplete OAuth authorization attempts do not create persistent tokens
- Social Profiles page displays `[Connect]` for unconnected OAuth accounts

---

## Actual Result

- Incomplete OAuth authorization attempts create persistent OAuth tokens
- Social Profiles page incorrectly displays `[Disconnect]` for unconnected networks

---

## Root Cause

Confirmed via DevTools — the bug is specific to the Social Network Sign Up flow and is not triggered by email/password registration:

```json
"usersOAuthProviders": []  // email/password account — no OAuth tokens created
```

All three tokens share the same `providerId` — the Google account ID is incorrectly assigned across all providers:

```json
{"id": 7174, "providerId": "116712803100990261335", "providerName": "GITHUB"}
{"id": 7202, "providerId": "116712803100990261335", "providerName": "LINKEDIN"}
{"id": 7207, "providerId": "116712803100990261335", "providerName": "GOOGLE"}
```

The OAuth flow creates and persists a token even when the user closes the authorization window before completing authentication. The token is tied to the initiating provider's ID rather than the target provider, resulting in ghost connections with incorrect provider identity.

---

## Additional Notes

Disconnecting the only connected OAuth provider (Google) is permitted without a warning or confirmation dialog. The token is recreated upon next Google login so no functional lockout occurs — however, this behavior may confuse users who disconnect their only authentication method without understanding the consequences.