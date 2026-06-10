# HMP-20 — Long Press Additional Menu Does Not Appear for Chat Items Without Online Status Indicator

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

- User is logged in
- User has multiple chats in Chat Selector
- At least one chat displays online status and one does not

---

## Steps to Reproduce

1. Navigate to the Chats screen
2. Long press a Chat Selector item displaying online status
3. Observe — additional menu appears with "Archive Chat" and "Delete Chat" options
4. Long press a Chat Selector item with no online status displayed
5. Observe — additional menu does not appear

---

## Expected Result

Additional menu with "Archive Chat" and "Delete Chat" options appears for all Chat Selector items regardless of online status indicator presence.

---

## Actual Result

- Additional menu appears only for chats where an online status indicator is displayed
- Background darkens for all chats — long press gesture is registered correctly
- "Archive Chat" and "Delete Chat" options are inaccessible for affected chats

---

## Additional Notes

- Online status includes both currently online and historical statuses, e.g. `2 days ago`
- Long press gesture is correctly registered for all chats — background darkens in all cases
- Issue is not related to chat age — older chats with online status still display the menu correctly
- Issue is not related to user profile status — Active profiles without online status are also affected