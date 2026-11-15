# Error handling

> **English** | [Tiếng Việt](error-handling.vi.md)

---

When a request fails, the API returns an error envelope:

```json
{ "status": "error", "code": "MESSAGING_WINDOW_CLOSED", "message": "..." }
```

Base your logic on `code` (stable) rather than `message` (subject to change). The SDKs map every `code`/HTTP status to an exception class.

## HTTP codes

| HTTP | Meaning |
|---|---|
| 200 / 201 | Success / Created |
| 400 | Invalid parameters |
| 401 | Missing or invalid access token |
| 403 | Missing scope or no permission on the channel |
| 404 | Resource not found |
| 409 | Conflict (duplicate `idempotency_key`) |
| 422 | Failed business validation |
| 429 | Rate limit exceeded |
| 5xx | Server error |

## Error code strings → exception classes

| `code` | HTTP | Python | Node / Java |
|---|---|---|---|
| `UNAUTHORIZED` | 401 | `AuthenticationError` | `AuthenticationError` / `AuthenticationException` |
| `INSUFFICIENT_SCOPE` | 403 | `AuthorizationError` | `AuthorizationError` / `AuthorizationException` |
| `CHANNEL_NOT_CONNECTED` | 403 | `AuthorizationError` | `AuthorizationError` / `AuthorizationException` |
| `NOT_FOUND` | 404 | `NotFoundError` | `NotFoundError` / `NotFoundException` |
| `VALIDATION_ERROR` | 400/422 | `ValidationError` | `ValidationError` / `ValidationException` |
| `MESSAGING_WINDOW_CLOSED` | 422 | `MessagingWindowClosedError` | `MessagingWindowClosedError` / `MessagingWindowClosedException` |
| `DUPLICATE_REQUEST` | 409 | `ConflictError` | `ConflictError` / `ConflictException` |
| `TOO_MANY_REQUESTS` | 429 | `RateLimitError` | `RateLimitError` / `RateLimitException` |
| `INTERNAL_ERROR` | 500 | `ServerError` | `ServerError` / `ServerException` |

All of them inherit from `OmnichannelError` (Python/Node) / `OmnichannelException` (PHP/Java). Go uses a single `*APIError` type with `Kind()`.

## Examples

### Python
```python
from hitechcloud_omnichannel import MessagingWindowClosedError, RateLimitError, OmnichannelError
import time

try:
    omni.messages.send_text("cnv_01HZABCDEF", "...")
except MessagingWindowClosedError:
    pass  # outside the 24h window -> use a template
except RateLimitError as e:
    time.sleep(e.retry_after or 5)
except OmnichannelError as e:
    print(e.code, e.message)
```

### Go
```go
_, err := c.Messages.SendText("cnv_01HZABCDEF", "...", "")
if apiErr, ok := err.(*omnichannel.APIError); ok {
    switch apiErr.Kind() {
    case omnichannel.KindMessagingWindow: /* use a template */
    case omnichannel.KindRateLimit:       time.Sleep(time.Duration(apiErr.RetryAfter) * time.Second)
    }
}
```

## Retries

The SDKs retry with exponential backoff on `429` (honouring `Retry-After`) and `5xx`. For `400/401/403/404/422`, retrying is usually pointless — fix the request instead.
