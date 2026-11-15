# HiTechCloud Omnichannel — Go SDK

> **English** | [Tiếng Việt](README.vi.md)

---

Official Go SDK for the [HiTechCloud Omnichannel API](https://developers.hitechcloud.vn).

## Installation

```bash
go get github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/go
```

```go
import omnichannel "github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/go"
```

## Setup

```go
c := omnichannel.NewClient(
    omnichannel.WithAccessToken(os.Getenv("OMNI_ACCESS_TOKEN")),
    // omnichannel.WithSandbox(),
)
```

## Quick example

```go
// List channels
env, err := c.Channels.List(omnichannel.Params(map[string]string{"platform": "facebook"}))

// Send a message
_, err = c.Messages.SendText("cnv_01HZABCDEF", "Hello!", "")

// Tag + assign
_, _ = c.Conversations.AddTags("cnv_01HZABCDEF", []string{"da-chot-don"})
_, _ = c.Conversations.Assign("cnv_01HZABCDEF", map[string]interface{}{
    "assignee_ids": []string{"usr_01HZNV01"},
})

// Create an order
_, _ = c.Orders.Create(map[string]interface{}{
    "conversation_id": "cnv_01HZABCDEF",
    "items": []map[string]interface{}{
        {"name": "Ao thun", "quantity": 2, "price": 150000},
    },
})
```

## Reading data from the envelope

```go
env, _ := c.Channels.List(nil)
var channels []map[string]interface{}
_ = env.DecodeData(&channels)
```

## Webhooks (HMAC-SHA256, header `X-HTC-Signature`)

```go
func handler(w http.ResponseWriter, r *http.Request) {
    raw, _ := io.ReadAll(r.Body)
    sig := r.Header.Get(omnichannel.SignatureHeader)
    if !omnichannel.VerifySignature(raw, sig, signingSecret) {
        http.Error(w, "invalid signature", http.StatusBadRequest)
        return
    }
    // handle idempotently by event_id
    w.WriteHeader(http.StatusOK)
}
```

## Error handling

```go
_, err := c.Messages.SendText("cnv_01HZABCDEF", "...", "")
if apiErr, ok := err.(*omnichannel.APIError); ok {
    switch apiErr.Kind() {
    case omnichannel.KindMessagingWindow:
        // use a template
    case omnichannel.KindRateLimit:
        time.Sleep(time.Duration(apiErr.RetryAfter) * time.Second)
    }
}
```

## License

MIT
