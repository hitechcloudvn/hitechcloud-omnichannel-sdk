# Contributing

> **English** | [Tiếng Việt](CONTRIBUTING.vi.md)

---

Thank you for your interest in contributing!

## Environment setup

1. Clone this repository.
2. Pick the language SDK under `sdk/`.
3. Install dependencies and run the tests.

| Language | Install | Test |
|---|---|---|
| Python | `pip install -e sdk/python[dev]` | `pytest sdk/python` |
| Node.js | `cd sdk/nodejs && npm install && npm run build` | `npm test` |
| PHP | `cd sdk/php && composer install` | `vendor/bin/phpunit` |
| Go | `cd sdk/go` | `go test ./...` |
| Java | `cd sdk/java` | `mvn test` |

## Guidelines

- Every method must map to a real endpoint in `openapi.json`.
- Keep the resource structure consistent across languages (channels, conversations, messages, media, customers, tags, users, orders, webhooks, widget, stats, sandbox, account).
- Add docstring/JSDoc/PHPDoc for public methods.
- Follow each language's conventions; run the formatter (`gofmt`, `prettier`, ...).

## Adding a new language

1. Create a directory under `sdk/<language>` with Client + Resources + errors + webhook verify + README + manifest.
2. Add basic tests (client construction + webhook signature verification).
3. Add examples under `examples/<language>`.
4. Update `README.md` and `CHANGELOG.md`.

## Pull requests

1. Fork and create a feature branch.
2. Add tests for your changes.
3. Make sure all tests pass.
4. Open a PR with a clear description of the change.

## License

By contributing, you agree that your contributions are licensed under the MIT license.
