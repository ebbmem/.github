<p align="left">
  <a href="https://ebbmem.ai">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://ebbmem.ai/brand/lockup-dark.svg">
      <source media="(prefers-color-scheme: light)" srcset="https://ebbmem.ai/brand/lockup-light.svg">
      <img alt="ebbmem" src="https://ebbmem.ai/brand/lockup-light.svg" height="40">
    </picture>
  </a>
</p>

**Memory that remembers what matters, and forgets the rest.** — The cognitive memory engine for AI agents.

[![Website](https://img.shields.io/badge/website-ebbmem.ai-F59E0B?style=flat-square)](https://ebbmem.ai)
[![Docs](https://img.shields.io/badge/docs-ebbmem.ai%2Fdocs-0EA5E9?style=flat-square)](https://ebbmem.ai/docs)
[![Go SDK](https://img.shields.io/github/v/tag/ebbmem/ebbmem-go?style=flat-square&label=go%20%7C%20ebbmem-go)](https://pkg.go.dev/github.com/ebbmem/ebbmem-go)
[![PyPI](https://img.shields.io/pypi/v/ebbmem-client?style=flat-square&label=pypi%20%7C%20ebbmem-client)](https://pypi.org/project/ebbmem-client/)
[![Crates.io](https://img.shields.io/crates/v/ebbmem-client?style=flat-square&label=crates.io%20%7C%20ebbmem-client)](https://docs.rs/crate/ebbmem-client/0.2.0)
[![License: Proprietary](https://img.shields.io/badge/license-Proprietary-6B7280?style=flat-square)](https://github.com/ebbmem/ebbmem-go/blob/main/LICENSE)

---

## What is ebbmem?

Standard AI agent memory stores keep every trace at equal weight until stale, low-value context crowds out what matters at recall time. **ebbmem** is a hosted cognitive memory engine for AI agents, accessible over a REST/HTTP API, streamable HTTP Model Context Protocol (`POST /mcp`), and multi-language client SDKs. Modeled on how biological memory behaves, ebbmem uses a mass-dilated Ebbinghaus forgetting curve so unused episodic traces decay over time, every recall refreshes what it touches, and repeatedly accessed episodes consolidate into durable semantic facts.

## Key Capabilities

- **Four-Tier Memory Model** — Store memories across `working`, `episodic`, `semantic`, and `procedural` tiers (`POST /api/v1/memory/remember` and `/api/v1/memory/remember/batch`), promoting time-stamped conversation and tool-call episodes into stable semantic facts linked to their sources.
- **Mass-Dilated Ebbinghaus Decay & Consolidation** — Retrievability falls as a memory ages so older traces stop crowding recall; recalled memories are refreshed, and repeatedly recalled traces consolidate into higher-stability semantic knowledge.
- **Hybrid & Profiled Recall** — Hybrid recall over episodes and facts ranked by relevance and current retention across tunable recall profiles (`Balanced`, `FactualPrecision`, `CreativeExploration`, `DebugDiagnostic`), plus federated recall (`recall_federated`), reinforcement (`reinforce`), reflection (`reflect`), and diagnostics (`why_not`, `status`).
- **Tenant-Scoped Namespaces** — Isolate memories per user, agent, or tenant via the `X-Ebb-Namespace` header or JSON request body (`{tenant}:default` isolation with `403` enforcement across tenant boundaries).
- **Hosted HTTP API, MCP & SSE Streams** — Connect agents via streamable HTTP JSON-RPC MCP (`POST /mcp`, legacy SSE at `GET /mcp`), REST routes (`/api/v1/memory/*`, `/api/v1/tasks/*`, `/healthz`), Server-Sent Events (`GET /api/v1/events/stream`), and OpenAPI specs ([API Reference](https://ebbmem.ai/docs/api), [Redoc](https://ebbmem.ai/docs/api/redoc), [Swagger UI](https://ebbmem.ai/docs/api/swagger)).
- **Client SDKs (`v0.2.0`)** — Shared client contract across [Go](https://ebbmem.ai/docs/sdks/go), [Python](https://ebbmem.ai/docs/sdks/python), [Rust](https://ebbmem.ai/docs/sdks/rust), [TypeScript](https://ebbmem.ai/docs/sdks/typescript), [Java](https://ebbmem.ai/docs/sdks/java), and [C#](https://ebbmem.ai/docs/sdks/csharp) with environment-driven auth (`EBBMEM_ORIGIN`, `EBBMEM_TOKEN`, `EBBMEM_API_KEY`, `EBBMEM_NAMESPACE`), cursor pagination, typed `EBB-…` error envelopes, idempotency keys, and exponential-backoff retries on `429` (`Retry-After`) and `502`/`503`/`504`.
- **Published Benchmarks** — **94.0%** on MindSpan QA (`376/400`), **91.4%** on LoCoMo QA (`1,408/1,540`, categories 1–4), and **98.6%** session hit@10 / **95.7%** turn hit@10 on keyless LongMemEval retrieval (fully local, zero LLM calls; see [Benchmarks & Methodology](https://ebbmem.ai/docs#benchmarks)).

## Repositories

| Repository | Description | Language / SDK |
| :--- | :--- | :--- |
| [`ebbmem/ebbmem-go`](https://github.com/ebbmem/ebbmem-go) | Go client for the Ebb memory HTTP API (`v0.2.0`) with context-aware calls, cursor pagination, SSE streaming, and automatic retries. | Go (`github.com/ebbmem/ebbmem-go`) |
| [`ebbmem/.github`](https://github.com/ebbmem/.github) | Organization profile and community configuration for the `ebbmem` GitHub organization. | Markdown |

Published client packages are also available on [pkg.go.dev (`github.com/ebbmem/ebbmem-go`)](https://pkg.go.dev/github.com/ebbmem/ebbmem-go), [PyPI (`ebbmem-client`)](https://pypi.org/project/ebbmem-client/), and [crates.io / docs.rs (`ebbmem-client`)](https://docs.rs/crate/ebbmem-client/0.2.0). Full SDK documentation is at [ebbmem.ai/docs/sdks](https://ebbmem.ai/docs/sdks).

## Quick Start

Set `EBBMEM_ORIGIN` to your hosted origin and `EBBMEM_TOKEN` to a Bearer token (or `EBBMEM_API_KEY` for `X-API-Key`).

### Go (`ebbmem/ebbmem-go`)

```bash
go get github.com/ebbmem/ebbmem-go@v0.2.0
```

```go
client, err := ebbmem.NewClient() // reads EBBMEM_ORIGIN and EBBMEM_TOKEN
stored, err := client.Memory.Remember(ctx, "the harbor light is cobalt", nil, nil, nil, "")
found, err := client.Memory.Recall(ctx, "harbor light", nil, nil)
```

### Python (`ebbmem-client`)

```bash
pip install ebbmem-client
```

```python
from ebbmem_client import EbbmemClient

client = EbbmemClient()  # reads EBBMEM_ORIGIN and EBBMEM_TOKEN
stored = client.memory.remember("the harbor light is cobalt")
found = client.memory.recall("harbor light")
```

### HTTP API (`curl`)

```bash
curl -sS -X POST "${EBBMEM_ORIGIN}/api/v1/memory/remember" \
  -H "Authorization: Bearer ${EBBMEM_TOKEN}" \
  -H "Content-Type: application/json" \
  -H "X-Ebb-Namespace: default" \
  -d '{"content":"User prefers metric units","tier":"episodic","importance":0.8}'

curl -sS -X POST "${EBBMEM_ORIGIN}/api/v1/memory/recall" \
  -H "Authorization: Bearer ${EBBMEM_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"query":"metric units","limit":5,"profile":"Balanced"}'
```

## Links & Resources

- **Website**: [ebbmem.ai](https://ebbmem.ai) ([Pricing](https://ebbmem.ai/pricing) · [Enterprise](https://ebbmem.ai/enterprise))
- **Documentation**: [ebbmem.ai/docs](https://ebbmem.ai/docs) ([Quickstart](https://ebbmem.ai/docs#quickstart) · [Client SDKs](https://ebbmem.ai/docs/sdks) · [Benchmarks](https://ebbmem.ai/docs#benchmarks) · [Methodology](https://ebbmem.ai/docs#methodology))
- **API Reference**: [Overview](https://ebbmem.ai/docs/api) · [Redoc](https://ebbmem.ai/docs/api/redoc) · [Swagger UI](https://ebbmem.ai/docs/api/swagger) · [OpenAPI JSON](https://ebbmem.ai/openapi.json) · [OpenAPI YAML](https://ebbmem.ai/openapi.yaml)
- **Community & Contributing**: Open issues or pull requests on [`ebbmem/ebbmem-go`](https://github.com/ebbmem/ebbmem-go/issues), or reach out via [ebbmem.ai/contact](https://ebbmem.ai/contact) and [support@ebbmem.ai](mailto:support@ebbmem.ai).
- **Security**: To report a security issue privately, email [support@ebbmem.ai](mailto:support@ebbmem.ai) or [ebbmem@spectrayan.com](mailto:ebbmem@spectrayan.com).
- **License**: Client SDKs and hosted service are proprietary — Copyright © Spectrayan. All rights reserved. See [`ebbmem/ebbmem-go/LICENSE`](https://github.com/ebbmem/ebbmem-go/blob/main/LICENSE).

---

Built by [Spectrayan](https://github.com/spectrayan)
