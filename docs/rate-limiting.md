# Pagination & rate limiting

> **English** | [Tiếng Việt](rate-limiting.vi.md)

---

## Cursor pagination

Used for `conversations` and `messages`. Omit `cursor` on the first call; pass back `next_cursor` on subsequent calls until `has_more` is `false`.

```python
cursor = None
while True:
    body = omni.conversations.list(channel_id="chn_01HZX8QWERTY", per_page=60, cursor=cursor)
    for cv in body["data"]:
        ...
    pg = body["meta"]["pagination"]
    if not pg["has_more"]:
        break
    cursor = pg["next_cursor"]
```

Built-in helpers: `omni.conversations.iterate(...)` (Python) and `for await (const c of omni.conversations.iterate(...))` (Node) walk every page for you.

## Page pagination

Used for `channels`, `customers`, `orders`, `posts`, `call-logs`.

| Parameter | Default | Description |
|---|---|---|
| `page` | 1 | Page number |
| `per_page` | 50 | Records per page (max 100; some endpoints cap at 30/60) |

## Rate limits

| Type | Limit | Header |
|---|---|---|
| Default per token | 120 req/min | `X-RateLimit-Limit: 120` |
| Messages per channel | 20 msg/sec | `X-RateLimit-Limit: 20` |
| Sandbox | 60 req/min | `X-RateLimit-Limit: 60` |

When you exceed a quota, the API returns HTTP `429` with a `Retry-After` header (seconds) and an envelope:

```json
{ "status": "error", "code": "TOO_MANY_REQUESTS", "message": "...", "retry_after": 18 }
```

The SDK waits for `Retry-After` and retries (within `max_retries`). The `RateLimitError` exception also exposes `retry_after` so you can throttle yourself if needed.
