# HiTechCloud Omnichannel — PHP SDK

> **English** | [Tiếng Việt](README.vi.md)

---

Official PHP SDK for the [HiTechCloud Omnichannel API](https://developers.hitechcloud.vn). PHP 8+, built on Guzzle.

## Installation

```bash
composer require hitechcloudvn/omnichannel-sdk
```

## Setup

```php
<?php
require 'vendor/autoload.php';

use HiTechCloud\Omnichannel\Client;

$omni = new Client(getenv('OMNI_ACCESS_TOKEN'));
// Sandbox: new Client(getenv('OMNI_ACCESS_TOKEN'), null, true);
```

## Quick example

```php
// List channels
$channels = $omni->channels()->list(['platform' => 'facebook', 'connected' => true]);

// Unread conversations
$convs = $omni->conversations()->list([
    'channel_id' => 'chn_01HZX8QWERTY',
    'unread_first' => true,
]);

// Send a message
$omni->messages()->sendText('cnv_01HZABCDEF', 'Hello!');

// Tag + assign
$omni->conversations()->addTags('cnv_01HZABCDEF', ['da-chot-don']);
$omni->conversations()->assign('cnv_01HZABCDEF', ['usr_01HZNV01']);

// Create an order
$omni->orders()->create('cnv_01HZABCDEF', [
    ['name' => 'Ao thun', 'quantity' => 2, 'price' => 150000],
]);
```

## Webhooks (HMAC-SHA256, header `X-HTC-Signature`)

```php
use HiTechCloud\Omnichannel\Webhook;

$raw = file_get_contents('php://input');
$sig = $_SERVER['HTTP_X_HTC_SIGNATURE'] ?? null;

if (!Webhook::verifySignature($raw, $sig, getenv('SIGNING_SECRET'))) {
    http_response_code(400);
    exit('invalid signature');
}
$event = json_decode($raw, true);
// handle idempotently by $event['event_id']
http_response_code(200);
```

## Error handling

```php
use HiTechCloud\Omnichannel\RateLimitException;
use HiTechCloud\Omnichannel\MessagingWindowClosedException;

try {
    $omni->messages()->sendText('cnv_01HZABCDEF', '...');
} catch (MessagingWindowClosedException $e) {
    // outside the 24h window -> use a template
} catch (RateLimitException $e) {
    sleep($e->getRetryAfter() ?? 5);
}
```

## License

MIT
