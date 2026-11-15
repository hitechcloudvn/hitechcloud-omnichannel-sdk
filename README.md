# HiTechCloud Omnichannel SDK

> **English** | [Tiếng Việt](README.vi.md)

---

[![CI](https://github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/actions/workflows/ci.yml/badge.svg)](https://github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> Official multi-language SDK for the [HiTechCloud Omnichannel API](https://developers.hitechcloud.vn) — brings **Facebook, Instagram, Zalo, TikTok and Website** conversations into one unified REST API: read, reply, tag, assign agents, close orders and receive real-time webhooks.

## Overview

Instead of integrating Facebook Graph API, Zalo OA API, TikTok Business and the website chat widget separately, you connect once and use one unified REST API. This repository provides SDKs for five languages, an OpenAPI specification, documentation in English and Vietnamese, and an MCP server.

| Item | Value |
|---|---|
| Production host | `https://omni.hitechcloud.vn` |
| Sandbox host | `https://omni-sandbox.hitechcloud.vn` |
| Base path | `/v1` |
| Authentication | Bearer access token |
| Identifiers | ULID (`chn_…`, `cnv_…`, `msg_…`, `cus_…`, `ord_…`) |
| Response | Envelope `{ status, data, meta }` |
| Webhook | HMAC-SHA256 via the `X-HTC-Signature` header |

## Supported languages

| Language | Package | Path |
|---|---|---|
| Node.js / TypeScript | `@hitechcloudvn/omnichannel-sdk` (npm) | [`sdk/nodejs`](sdk/nodejs) |
| Python | `hitechcloud-omnichannel` (PyPI) | [`sdk/python`](sdk/python) |
| PHP | `hitechcloudvn/omnichannel-sdk` (Composer) | [`sdk/php`](sdk/php) |
| Go | `github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/go` | [`sdk/go`](sdk/go) |
| Java | `vn.hitechcloud:omnichannel-sdk` (Maven) | [`sdk/java`](sdk/java) |

## Resource groups

`channels` · `conversations` · `messages` · `media` · `customers` · `tags` · `users` · `orders` · `webhooks` · `widget` · `stats` · `sandbox` · `account`.

## Quick start

### Node.js / TypeScript
```ts
import { Omnichannel } from "@hitechcloudvn/omnichannel-sdk";

const omni = new Omnichannel({ accessToken: process.env.OMNI_ACCESS_TOKEN! });
const channels = await omni.channels.list({ platform: "facebook" });
await omni.messages.sendText("cnv_01HZABCDEF", "Hello!");
```

### Python
```python
from hitechcloud_omnichannel import Omnichannel

omni = Omnichannel(access_token="YOUR_ACCESS_TOKEN")
omni.channels.list(platform="facebook")
omni.messages.send_text("cnv_01HZABCDEF", "Hello!")
```

### PHP
```php
$omni = new HiTechCloud\Omnichannel\Client(getenv('OMNI_ACCESS_TOKEN'));
$omni->messages()->sendText('cnv_01HZABCDEF', 'Hello!');
```

### Go
```go
c := omnichannel.NewClient(omnichannel.WithAccessToken(os.Getenv("OMNI_ACCESS_TOKEN")))
c.Messages.SendText("cnv_01HZABCDEF", "Hello!", "")
```

### Java
```java
OmnichannelClient omni = new OmnichannelClient(System.getenv("OMNI_ACCESS_TOKEN"));
omni.messages().sendText("cnv_01HZABCDEF", "Hello!", null);
```

## Webhooks (X-HTC-Signature)

Every webhook request carries an `X-HTC-Signature` header: the HMAC-SHA256 of the request body, signed with your `signing_secret`. Each SDK ships a ready-made verification helper:

```ts
import { verifySignature } from "@hitechcloudvn/omnichannel-sdk";
if (!verifySignature(rawBody, req.header("X-HTC-Signature"), SIGNING_SECRET)) { /* 400 */ }
```

```python
from hitechcloud_omnichannel import verify_signature
verify_signature(raw_body, request.headers["X-HTC-Signature"], signing_secret)
```

Events: `message.created`, `message.updated`, `conversation.created`, `conversation.updated`, `order.created`, `order.updated`, `channel.connected`, `channel.disconnected`, `post.created`, `post.updated`, `subscription.updated`.

## Documentation

- [Quick start](docs/getting-started.md)
- [Authentication](docs/authentication.md)
- [API reference](docs/api-reference.md)
- [Error handling](docs/error-handling.md)
- [Pagination & rate limiting](docs/rate-limiting.md)
- [OpenAPI specification](openapi.json)
- [MCP server](mcp/README.md)

## License

[MIT](LICENSE) © HiTechCloud Vietnam
