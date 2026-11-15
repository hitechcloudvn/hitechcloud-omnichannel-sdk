# Tài liệu HiTechCloud Omnichannel SDK

> [English](README.md) | **Tiếng Việt**

---

SDK đa ngôn ngữ chính thức cho [HiTechCloud Omnichannel API](https://developers.hitechcloud.vn) — gom hội thoại Facebook, Instagram, Zalo, TikTok và Website vào một REST API hợp nhất.

## Mục lục

- [Bắt đầu nhanh](getting-started.vi.md)
- [Xác thực](authentication.vi.md)
- [Tham chiếu API](api-reference.vi.md)
- [Xử lý lỗi](error-handling.vi.md)
- [Phân trang & Rate limit](rate-limiting.vi.md)

## Liên kết

- [Đặc tả OpenAPI](../openapi.json)
- [Kho GitHub](https://github.com/hitechcloudvn/hitechcloud-omnichannel-sdk)
- [Tài liệu API](https://developers.hitechcloud.vn/tai-lieu/omnichannel/tong-quan)

## Thông tin chung

| Hạng mục | Giá trị |
|---|---|
| Host Production | `https://omni.hitechcloud.vn` |
| Host Sandbox | `https://omni-sandbox.hitechcloud.vn` |
| Base path | `/v1` |
| Xác thực | Bearer access token |
| Định danh | ULID (ví dụ `cnv_01HZABCDEF`) |
| Phản hồi | Envelope `{ status, data, meta }` |
| Webhook | Chữ ký HMAC-SHA256 qua header `X-HTC-Signature` |

## Ngôn ngữ hỗ trợ

| Ngôn ngữ | Gói |
|---|---|
| Node.js / TypeScript | `@hitechcloudvn/omnichannel-sdk` (npm) |
| Python | `hitechcloud-omnichannel` (PyPI) |
| PHP | `hitechcloudvn/omnichannel-sdk` (Composer) |
| Go | `github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/go` |
| Java | `vn.hitechcloud:omnichannel-sdk` (Maven) |
