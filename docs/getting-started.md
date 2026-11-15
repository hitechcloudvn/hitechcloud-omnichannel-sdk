# Quick start

> **English** | [Tiếng Việt](getting-started.vi.md)

---

This guide takes you from zero to sending your first customer reply through the HiTechCloud Omnichannel API.

## 1. Get an access token

1. Sign in to `https://omni.hitechcloud.vn`.
2. Go to **Settings → API & Integration → Access Token**.
3. Click **+ Create new token**, choose the scopes, then click Create.
4. Copy the `access_token` and store it in an environment variable. The token is shown only once.

> An access token lets you read and reply to customer messages. Never embed it in frontend code or commit it to Git — call the API from the server side only.

## 2. Install the SDK

### Node.js / TypeScript
```bash
npm install @hitechcloudvn/omnichannel-sdk
```

### Python
```bash
pip install hitechcloud-omnichannel
```

### PHP
```bash
composer require hitechcloudvn/omnichannel-sdk
```

### Go
```bash
go get github.com/hitechcloudvn/hitechcloud-omnichannel-sdk/go
```

### Java (Maven)
```xml
<dependency>
  <groupId>vn.hitechcloud</groupId>
  <artifactId>omnichannel-sdk</artifactId>
  <version>1.0.0</version>
</dependency>
```

## 3. Make your first API call

### Node.js
```ts
import { Omnichannel } from "@hitechcloudvn/omnichannel-sdk";

const omni = new Omnichannel({ accessToken: process.env.OMNI_ACCESS_TOKEN! });
const channels = await omni.channels.list({ platform: "facebook" });
console.log(channels.data);
```

### Python
```python
from hitechcloud_omnichannel import Omnichannel
import os

omni = Omnichannel(access_token=os.getenv("OMNI_ACCESS_TOKEN"))
print(omni.channels.list(platform="facebook")["data"])
```

### PHP
```php
$omni = new HiTechCloud\Omnichannel\Client(getenv('OMNI_ACCESS_TOKEN'));
print_r($omni->channels()->list(['platform' => 'facebook']));
```

## 4. Send a reply

```python
omni.messages.send_text("cnv_01HZABCDEF", "Hello! How can HiTechCloud help you today?")
```

```ts
await omni.messages.sendText("cnv_01HZABCDEF", "Hello!");
```

## 5. Sandbox environment

Every SDK can switch to Sandbox (`https://omni-sandbox.hitechcloud.vn`) for safe experimentation without sending real messages:

- Node: `new Omnichannel({ accessToken, sandbox: true })`
- Python: `Omnichannel(access_token=..., sandbox=True)`
- PHP: `new Client($token, null, true)`
- Go: `omnichannel.NewClient(omnichannel.WithAccessToken(...), omnichannel.WithSandbox())`
- Java: `new OmnichannelClient(token, true)`

Next: [Authentication](authentication.md) · [API reference](api-reference.md) · [Error handling](error-handling.md).
