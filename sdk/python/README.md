# HiTechCloud Omnichannel — Python SDK

> **English** | [Tiếng Việt](README.vi.md)

---

Official Python SDK for the [HiTechCloud Omnichannel API](https://developers.hitechcloud.vn): brings Facebook, Instagram, Zalo, TikTok and Website conversations into one unified REST API.

## Installation

```bash
pip install hitechcloud-omnichannel
```

## Setup

```python
from hitechcloud_omnichannel import Omnichannel

omni = Omnichannel(access_token="YOUR_ACCESS_TOKEN")
# For testing: Omnichannel(access_token="...", sandbox=True)
```

## Quick example

```python
# List connected Facebook channels
channels = omni.channels.list(platform="facebook", connected=True)

# Read a channel's unread conversations
convs = omni.conversations.list(channel_id="chn_01HZX8QWERTY", unread_first=True)

# Send a reply message
omni.messages.send_text("cnv_01HZABCDEF", "Hello! How can HiTechCloud help you today?")

# Tag and assign
omni.conversations.add_tags("cnv_01HZABCDEF", ["da-chot-don"])
omni.conversations.assign("cnv_01HZABCDEF", assignee_ids=["usr_01HZNV01"])

# Create an order from the conversation
omni.orders.create(
    conversation_id="cnv_01HZABCDEF",
    items=[{"name": "Ao thun", "quantity": 2, "price": 150000}],
)

# Upload media then send an image
media = omni.media.upload("/duong-dan/anh.jpg", channel_id="chn_01HZX8QWERTY")
omni.messages.send_image("cnv_01HZABCDEF", [media["data"]["id"]])
```

## Cursor pagination

```python
for conv in omni.conversations.iterate(channel_id="chn_01HZX8QWERTY", per_page=60):
    print(conv["id"], conv["customer"]["name"])
```

## Webhooks (HMAC-SHA256, header `X-HTC-Signature`)

```python
from hitechcloud_omnichannel import verify_signature

@app.post("/htc/webhook")
def handler(request):
    raw = request.get_data(as_text=True)
    sig = request.headers.get("X-HTC-Signature")
    if not verify_signature(raw, sig, SIGNING_SECRET):
        return "invalid signature", 400
    event = request.json
    # handle idempotently by event["event_id"]
    return "", 200
```

## Error handling

```python
from hitechcloud_omnichannel import (
    RateLimitError, MessagingWindowClosedError, OmnichannelError,
)

try:
    omni.messages.send_text("cnv_01HZABCDEF", "...")
except MessagingWindowClosedError:
    pass  # outside the 24h window -> use a template
except RateLimitError as e:
    time.sleep(e.retry_after or 5)
except OmnichannelError as e:
    print(e.code, e.message)
```

## Resources

`channels`, `conversations`, `messages`, `media`, `customers`, `tags`, `users`, `orders`, `webhooks`, `widget`, `stats`, `sandbox`, `account`.

## License

MIT
