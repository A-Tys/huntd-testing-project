# HUNTD-70 — Social Profiles Buttons on Edit Profile Page Remain Disabled After Closing Authorization Window

**Severity:** Minor  
**Priority:** Medium

---

## Environment

| | |
|---|---|
| Browser | Microsoft Edge 148.0.3967.70 (64-bit), Google Chrome 148.0.7778.168 (64-bit) |
| OS | Windows 10 Pro |

---

## Preconditions

- User is logged in
- At least one social profile is not connected

---

## Steps to Reproduce

1. Navigate to [Social Profiles](https://huntd.tech/settings/social-profiles)
2. Click `[Connect]` for any social network (LinkedIn, GitHub, or Google)
3. Observe — authorization window opens and all Social Profiles buttons become disabled
4. Close the authorization window without completing authorization
5. Observe — buttons remain disabled
6. Refresh the page
7. Observe — buttons are re-enabled

---

## Expected Result

Social Profiles buttons are re-enabled automatically when the authorization window is closed without completing authorization.

---

## Actual Result

Buttons remain disabled after closing the authorization window. The button remains in the DOM with a `disabled` attribute and `is-disabled` class — neither is removed on the window close event. A page refresh is required to re-enable the buttons.

---

## Additional Notes

The issue reproduces consistently across Microsoft Edge and Google Chrome, confirming it is not browser-specific. The root cause is that the window close event does not trigger a state reset on the parent page — the disabled state applied on authorization window open is never reversed unless the page is reloaded.