# Usage examples

> **English** | [Tiếng Việt](README.vi.md)

---

Each subdirectory holds an example for one language. Before running, install the corresponding SDK and set the `OMNI_ACCESS_TOKEN` environment variable.

| Language | Files | Run |
|---|---|---|
| Node.js | `nodejs/basic_usage.js`, `nodejs/webhook_server.js` | `node basic_usage.js` |
| Python | `python/basic_usage.py` | `python basic_usage.py` |
| PHP | `php/basic_usage.php` | `php basic_usage.php` |
| Go | `go/basic_usage.go` | `go run basic_usage.go` |
| Java | `java/BasicUsage.java` | compile with `omnichannel-sdk` on the classpath |

All examples use Production. For safe experimentation, switch to Sandbox:

- Node: `new Omnichannel({ accessToken, sandbox: true })`
- Python: `Omnichannel(access_token=..., sandbox=True)`
- PHP: `new Client($token, null, true)`
- Go: `omnichannel.NewClient(omnichannel.WithAccessToken(...), omnichannel.WithSandbox())`
- Java: `new OmnichannelClient(token, true)`
