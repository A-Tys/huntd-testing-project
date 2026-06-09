# HUNTD-65 — Chat Records Persist in DB After Deletion and Remain Partially Accessible

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

- Both Recruiter and Candidate accounts are accessible for testing
- Chat is initiated between Recruiter and Candidate

---

## Steps to Reproduce

### Scenario 1 — Recruiter Deletes Chat

1. Log in as Recruiter → navigate to [Chats](https://huntd.tech/chats)
2. Open an existing chat and delete it via Kebab menu
3. Observe — chat disappears from Chat Selector
4. Log in as Candidate and send a message to the "deleted" chat
5. Observe — message sends successfully
6. Reload page as Recruiter — deleted chat reappears in Chatbox
7. Attempt to send a message as Recruiter
8. Observe — `SequelizeUniqueConstraintError`

### Scenario 2 — Candidate Deletes Chat

1. Log in as Candidate → navigate to [Chats](https://huntd.tech/chats)
2. Open an existing chat and delete it via Kebab menu
3. Observe — chat disappears from Chat Selector
4. Observe — chat menu and chatbox remain visible
5. Attempt to send a message
6. Observe — `SequelizeUniqueConstraintError`
7. Attempt to close chat — cannot be closed without opening another chat

---

## Expected Result

Chat is permanently deleted for both parties upon deletion. Messages cannot be sent to a deleted chat and the Chat UI is fully dismissed after deletion.

---

## Actual Result

- Chat menu and chatbox remain visible after deletion by Candidate
- Messages sent to a deleted chat by any party succeed despite deletion
- Message attempts trigger `SequelizeUniqueConstraintError`
- Deleted chat cannot be dismissed by Candidate without opening another chat
- After chat deletion, Recruiter cannot initiate a new chat with the same Candidate — `sendProfileConnectionRequest` mutation fails with `Cannot return null for non-nullable field Mutation.sendProfileConnectionRequest`

---

## Root Cause

The chat record persists in the database after deletion. When the other party sends a message to the deleted chat, the chat reappears on reload for the deleting party. Subsequent message attempts fail because the `(user_id, profile_connection_id)` pair already exists in the database, violating a unique constraint.

---

## DevTools Logs

### Recruiter — sendMessage mutation

Request:
```json
{
  "operationName": "sendMessage",
  "variables": {
    "profileConnectionId": 261843,
    "message": "Test Message 2"
  }
}
```

Response:
```json
{
  "errors": [{
    "message": "Validation error",
    "path": ["sendMessage"],
    "extensions": {
      "code": "INTERNAL_SERVER_ERROR",
      "exception": {
        "name": "SequelizeUniqueConstraintError",
        "parent": {
          "code": "23505",
          "detail": "Key (user_id, profile_connection_id)=(22844, 261843) already exists."
        }
      }
    }
  }],
  "data": null
}
```

### Candidate — sendMessage mutation

Request:
```json
{
  "operationName": "sendMessage",
  "variables": {
    "profileConnectionId": 261843,
    "message": "Test Message 3"
  }
}
```

Response:
```json
{
  "errors": [{
    "message": "Validation error",
    "path": ["sendMessage"],
    "extensions": {
      "code": "INTERNAL_SERVER_ERROR",
      "exception": {
        "name": "SequelizeUniqueConstraintError",
        "parent": {
          "code": "23505",
          "detail": "Key (user_id, profile_connection_id)=(22854, 261843) already exists."
        }
      }
    }
  }],
  "data": null
}
```