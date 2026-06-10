# HMP-21 — Enabled "Notifications for Chats" Toggle on Preferences Screen Does Not Trigger Push Notifications for New Chat Messages

**Severity:** Major  
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

- User is authenticated
- "Notifications for Chats" toggle is enabled
- Android system notifications permission is enabled for Huntd app

---

## Steps to Reproduce

1. Send a message from another account while app is open
2. Observe — no push notification displayed
3. Navigate away from the app — app running in background
4. Send a message from another account
5. Observe — no push notification in status bar
6. Completely close the Huntd app
7. Send a message from another account
8. Observe — no push notification in status bar

---

## Expected Result

Push notification displayed in device status bar when a new chat message is received with "Notifications for Chats" toggle enabled.

---

## Actual Result

No push notifications displayed in any app state. Toggle appears to be a non-functional UI element. User must manually open the app to check for new messages — no workaround available.

---

## Root Cause

Confirmed via ADB logcat — no notification, FCM, or Firebase related logs detected when a message is received. The toggle UI responds to tap but does not trigger any underlying notification registration or delivery logic.

---

## Additional Notes

Tested across all app states:

| App State | Push Notification |
|---|:---:|
| Open (foreground) | ❌ |
| Background | ❌ |
| Completely closed | ❌ |

Android system notifications permission confirmed enabled for the Huntd app — the issue is not caused by OS-level permission restrictions.