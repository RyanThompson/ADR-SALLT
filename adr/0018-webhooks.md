# 0018 Webhooks

**Status:** Accepted · **Layer:** Laravel · **Principles:** P4 P8 · **Stack:** 0001

> Outbound webhooks follow Standard Webhooks v1 exactly.

## Rules
1. Body is `{"type","timestamp","data"}`. Event time is ISO 8601 UTC. Tenant and facts live under `data`.
2. Every attempt sends `webhook-id`, `webhook-timestamp`, `webhook-signature` (`v1,<base64>`) and `Content-Type: application/json`.
3. Sign `{msg_id}.{timestamp}.{raw_body}` with HMAC-SHA256 using the base64-decoded `whsec_` secret.
4. New secrets are `whsec_` + base64 of at least 24 random bytes.
5. `webhook-id` is stable across retries; only timestamp and signature change.
6. Stored payloads are normalized before encoding; an empty `{}` from a failed cast is a bug.
7. Fan-out starts from domain events; signing lives in one support class.

## Shape
```php
Http::withHeaders([
    'webhook-id' => $msgId,
    'webhook-timestamp' => (string) $attemptAt,
    'webhook-signature' => StandardWebhookSigner::sign($msgId, $attemptAt, $body, $secret),
])->withBody($body, 'application/json')->post($url);
```

## Not
- Custom signature headers.
- Signing the body alone.

Checklist: [`checklists/0018-webhooks.yml`](../checklists/0018-webhooks.yml)
