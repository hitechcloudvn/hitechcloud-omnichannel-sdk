# Changelog

> **English** | [Tiếng Việt](CHANGELOG.vi.md)

---

All notable changes to the HiTechCloud Omnichannel SDK are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-11-01

### Added
- Initial release of the multi-language SDK: Node.js/TypeScript, Python, PHP, Go, Java.
- Full coverage of the Omnichannel API `/v1`: channels, conversations, messages (text/image/file/comment_reply/private_reply/template), media upload, customers + notes, tags, agent assignment, automatic (round-robin) routing, orders, posts, web widget, call logs, data export, statistics, and webhook management.
- HMAC-SHA256 (`X-HTC-Signature`) webhook signature verification helpers for all five languages.
- Automatic retries with backoff for `429`/`5xx`, honouring `Retry-After`.
- Error mapping from envelope `code` to language-specific exception classes.
- Sandbox environment support (`omni-sandbox.hitechcloud.vn`).
- OpenAPI 3.0 specification, Vietnamese documentation, examples for every language, and a (Node) MCP server.

[1.0.0]: https://github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/releases/tag/v1.0.0
