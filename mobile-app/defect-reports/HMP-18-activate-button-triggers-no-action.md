# HMP-18 — [Activate Profile] Button on Recruiter Preview Profile Screen Does Not Trigger Profile Activation

**Severity:** Critical  
**Priority:** High

---

## Environment

| | |
|---|---|
| Device | Motorola Edge 30 Fusion |
| OS | Android 14 |
| App | Huntd Mobile Version 1.0.9 |

---

## Preconditions

User has completed Recruiter registration flow.

---

## Steps to Reproduce

1. Navigate to the Recruiter Profile screen
2. Observe — profile status is "Draft"
3. Tap `[Activate Profile]`

---

## Expected Result

Tapping `[Activate Profile]` triggers profile activation.

---

## Actual Result

- Button tap animation fires, but no profile activation is triggered
- Profile status remains "Draft"
- No error message displayed

---

## Root Cause

Confirmed via ADB logcat — only hardware rendering logs fire on button tap. No business logic or network events are triggered, indicating the button's `onPress` handler is not wired up or not firing:

```
QC2C2DEngine: calcYSize: unsupported or RGB color format, RGBA8888_UBWC
```

No GraphQL mutation or API call observed in logcat output following the tap.