# HiTechCloud Omnichannel — Java SDK

> **English** | [Tiếng Việt](README.vi.md)

---

Official Java SDK for the [HiTechCloud Omnichannel API](https://developers.hitechcloud.vn). Java 11+, built on `java.net.http` and Gson.

## Installation (Maven)

```xml
<dependency>
    <groupId>vn.hitechcloud</groupId>
    <artifactId>omnichannel-sdk</artifactId>
    <version>1.0.0</version>
</dependency>
```

## Setup

```java
import vn.hitechcloud.omnichannel.OmnichannelClient;

OmnichannelClient omni = new OmnichannelClient(System.getenv("OMNI_ACCESS_TOKEN"));
// Sandbox: new OmnichannelClient(token, true);
```

## Quick example

```java
import vn.hitechcloud.omnichannel.resources.BaseResource.Q;
import java.util.List;
import java.util.Map;

// List channels
Map<String, Object> channels = omni.channels().list(Q.of().p("platform", "facebook").build());

// Send a message
omni.messages().sendText("cnv_01HZABCDEF", "Hello!", null);

// Tag + assign
omni.conversations().addTags("cnv_01HZABCDEF", List.of("da-chot-don"));
omni.conversations().assign("cnv_01HZABCDEF", Map.of("assignee_ids", List.of("usr_01HZNV01")));

// Create an order
omni.orders().create("cnv_01HZABCDEF",
    List.of(Map.of("name", "Ao thun", "quantity", 2, "price", 150000)), null, null);
```

## Webhooks (HMAC-SHA256, header `X-HTC-Signature`)

```java
import vn.hitechcloud.omnichannel.Webhook;

String raw = /* raw request body */;
String sig = request.getHeader(Webhook.SIGNATURE_HEADER);
if (!Webhook.verifySignature(raw, sig, signingSecret)) {
    // 400 Bad Request
}
```

## Error handling

```java
import vn.hitechcloud.omnichannel.MessagingWindowClosedException;
import vn.hitechcloud.omnichannel.RateLimitException;

try {
    omni.messages().sendText("cnv_01HZABCDEF", "...", null);
} catch (MessagingWindowClosedException e) {
    // outside the 24h window -> use a template
} catch (RateLimitException e) {
    Thread.sleep((e.getRetryAfter() == null ? 5 : e.getRetryAfter()) * 1000L);
}
```

## License

MIT
