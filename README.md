# AI Gateway

A fork of [envoyproxy/ai-gateway](https://github.com/envoyproxy/ai-gateway) — an Envoy-based gateway for routing and managing AI/LLM API traffic.

## Overview

AI Gateway provides a unified proxy layer for interacting with multiple AI providers (OpenAI, AWS Bedrock, Google Gemini, Ollama, etc.) through a single Envoy-compatible interface. It handles:

- **Request routing** — Route requests to different AI backends based on model name, headers, or other attributes
- **Token-based rate limiting** — Track and limit usage by token counts across providers
- **Request/response transformation** — Normalize requests and responses across different provider APIs
- **Authentication** — Manage API keys and credentials per backend

## Architecture

```
Client → Envoy Proxy → AI Gateway Filter → AI Provider (OpenAI / Bedrock / Gemini / Ollama)
```

The AI Gateway runs as an Envoy external processor (ext_proc) sidecar, intercepting HTTP requests and responses to apply transformations, routing decisions, and usage tracking.

## Prerequisites

- Go 1.22+
- Envoy proxy (see `.envoy-version` for the required version)
- Docker (for local development)

## Getting Started

### Local Development with Ollama

1. Copy and configure the Ollama environment:
   ```bash
   cp .env.ollama .env
   # Edit .env with your settings
   ```

2. Start the gateway:
   ```bash
   make run
   ```

### Configuration

See the `examples/` directory for sample configurations for each supported provider.

> **Personal note:** I've been primarily testing with the Ollama provider locally using `llama3` and `mistral` models. The `examples/ollama/` config works out of the box with minimal changes.
>
> **Tip:** When running Ollama on Apple Silicon, make sure to set `OLLAMA_HOST=0.0.0.0` in your environment so Envoy can reach it from within Docker. Took me a while to figure this one out.

## Supported Providers

| Provider       | Status |
|----------------|--------|
| OpenAI         | ✅     |
| AWS Bedrock    | ✅     |
| Google Gemini  | ✅     |
| Ollama (local) | ✅     |

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting pull requests.

When filing issues, use the templates provided in `.github/ISSUE_TEMPLATE/`.

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.
