A Zap is built in Zapier's editor and cannot be committed as code, so this is the written spec of the one that is live, for anyone who wants to rebuild it.

**Name:** Lead intake: webhook to receiver. **Plan used:** Zapier's 14-day trial of paid features, because the Webhooks app is a premium app.

| Step | App and event | Settings |
|---|---|---|
| 1. Trigger | Webhooks by Zapier, **Catch Hook** | No child key. Zapier gives a unique URL; a sample lead was POSTed to it so the editor could read the fields (name, email, message, source) |
| 2. Action | Webhooks by Zapier, **POST** | URL: `<receiver>/ingest`. Payload type: **Json**. Wrap request in array: No. Unflatten: Yes |

**Data mapping in step 2:**

| Key | Value |
|---|---|
| `name` | mapped from step 1, Name |
| `email` | mapped from step 1, Email |
| `message` | mapped from step 1, Message |
| `source` | the fixed text `zapier-demo` (the caller's own `source` is deliberately not passed through) |

**Header:** `x-chain-key` set to the receiver's shared key, which the receiver compares before it stores or returns anything.

**Tested two ways:** the editor's own "Test step" (a real POST, the receiver stored it), and the published Zap fired from outside with `scripts/e2e_check.py`, which then reads the lead back from the receiver. The run shows as Successful in Zap history (`docs/zap-history.png`).
