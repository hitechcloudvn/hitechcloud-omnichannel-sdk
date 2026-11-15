# HiTechCloud Omnichannel SDK Documentation

> **English** | [Tiếng Việt](README.vi.md)

---

Official multi-language SDK for the [HiTechCloud Omnichannel API](https://developers.hitechcloud.vn) — brings Facebook, Instagram, Zalo, TikTok and Website conversations into one unified REST API.

## Table of contents

- [Quick start](getting-started.md)
- [Authentication](authentication.md)
- [API reference](api-reference.md)
- [Error handling](error-handling.md)
- [Pagination & rate limiting](rate-limiting.md)

## Links

- [OpenAPI specification](../openapi.json)
- [GitHub repository](https://github.com/hitechcloudvn/hitechcloud-omnichannel-sdk)
- [API documentation](https://developers.hitechcloud.vn/tai-lieu/omnichannel/tong-quan)

## General

| Item | Value |
|---|---|
| Production host | `https://omni.hitechcloud.vn` |
| Sandbox host | `https://omni-sandbox.hitechcloud.vn` |
| Base path | `/v1` |
| Authentication | Bearer access token |
| Identifiers | ULID (e.g. `cnv_01HZABCDEF`) |
| Response | Envelope `{ status, data, meta }` |
| Webhook | HMAC-SHA256 signature via the `X-HTC-Signature` header |

## Supported languages

| Language | Package |
|---|---|
| Node.js / TypeScript | `@hitechcloudvn/omnichannel-sdk` (npm) |
| Python | `hitechcloud-omnichannel` (PyPI) |
| PHP | `hitechcloudvn/omnichannel-sdk` (Composer) |
| Go | `github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/go` |
| Java | `vn.hitechcloud:omnichannel-sdk` (Maven) |
