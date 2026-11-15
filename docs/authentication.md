# Authentication

> **English** | [Tiếng Việt](authentication.vi.md)

---

Every Omnichannel API endpoint requires a valid access token.

## Bearer token (recommended)

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
Accept: application/json
```

In the SDKs, the token is passed at client construction and attached to every request automatically:

```python
omni = Omnichannel(access_token="YOUR_ACCESS_TOKEN")
```

## `access_token` query parameter

For environments where you cannot set arbitrary headers (reverse webhooks, no-code tools), you can pass the token via the query string:

```bash
curl "https://omni.hitechcloud.vn/v1/channels?access_token=YOUR_ACCESS_TOKEN"
```

> Prefer the Bearer header. When using `access_token` in the URL, the token may end up in logs — only do this over trusted HTTPS with a narrowly scoped token.

## Scopes

| Scope | Allows |
|---|---|
| `channels:read` | Read the channel list and channel details |
| `conversations:read` | Read conversations and messages |
| `messages:write` | Send messages, reply to comments |
| `tags:write` | Add / remove conversation tags |
| `assign:write` | Assign conversations to agents |
| `customers:write` | Update customer profiles and notes |
| `orders:read` | Read orders created from conversations |
| `webhooks:write` | Register and manage webhooks |

## Channel access tokens

The master access token can mint a dedicated token per channel:

```bash
curl -X POST "https://omni.hitechcloud.vn/v1/channels/chn_01HZX8QWERTY/access-token" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{ "scopes": ["conversations:read", "messages:write"] }'
```

SDK: `omni.channels.create_access_token("chn_...", ["conversations:read", "messages:write"])`.

## Checking a token

`GET /v1/me` returns the token owner, its scopes and the channels it can access:

```python
print(omni.me())  # or omni.account.me()
```

When the token is invalid or expired, the API returns HTTP `401` with `code: UNAUTHORIZED`.
