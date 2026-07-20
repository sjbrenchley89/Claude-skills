# Claude Model Catalog

> **Cached: 2026-07-20**

**Only use exact model IDs listed in this file.** Never guess or construct model IDs — incorrect IDs will cause API errors. Use aliases wherever available. For the latest information, WebFetch the Models Overview URL in `shared/live-sources.md`, or query the Models API directly (see Programmatic Model Discovery below).

## Programmatic Model Discovery

For **live** capability data — context window, max output tokens, feature support (thinking, vision, effort, structured outputs, etc.) — query the Models API instead of relying on the cached tables below. Use this when the user asks "what's the context window for X", "does model X support vision/thinking/effort", "which models support feature Y", or wants to select a model by capability at runtime.

```python
m = client.models.retrieve("claude-opus-4-8")
m.id                 # "claude-opus-4-8"
m.display_name       # "Claude Opus 4.8"
m.max_input_tokens   # context window (int)
m.max_tokens         # max output tokens (int)

# capabilities is an untyped nested dict — bracket access, check ["supported"] at the leaf
caps = m.capabilities
caps["image_input"]["supported"]                       # vision
caps["thinking"]["types"]["adaptive"]["supported"]     # adaptive thinking
caps["effort"]["max"]["supported"]                     # effort: max (also low/medium/high)
caps["structured_outputs"]["supported"]
caps["context_management"]["compact_20260112"]["supported"]

# filter across all models — iterate the page object directly (auto-paginates); do NOT use .data
[m for m in client.models.list()
 if m.capabilities["thinking"]["types"]["adaptive"]["supported"]
 and m.max_input_tokens >= 200_000]
```

Top-level fields (`id`, `display_name`, `max_input_tokens`, `max_tokens`) are typed attributes. `capabilities` is a dict — use bracket access, not attribute access. The API returns the full capability tree for every model with `supported: true/false` at each leaf, so bracket chains are safe without `.get()` guards. TypeScript SDK: same method names, also auto-paginates on iteration.

### Raw HTTP

```bash
curl https://api.anthropic.com/v1/models/claude-opus-4-8 \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01"
```

## Current Models (recommended)

| Friendly Name     | Alias (use this)    | Full ID                       | Context        | Max Output | Status |
|-------------------|---------------------|-------------------------------|----------------|------------|--------|
| Claude Fable 5    | `claude-fable-5`    | —                             | 1M             | 128K       | Active |
| Claude Mythos 5   | `claude-mythos-5`   | —                             | 1M             | 128K       | Invitation-only (Project Glasswing) |
| Claude Opus 4.8   | `claude-opus-4-8`   | —                             | 1M             | 128K       | Active |
| Claude Opus 4.7   | `claude-opus-4-7`   | —                             | 1M             | 128K       | Active |
| Claude Opus 4.6   | `claude-opus-4-6`   | —                             | 1M             | 128K       | Active |
| Claude Sonnet 5   | `claude-sonnet-5`   | —                             | 1M             | 128K       | Active |
| Claude Haiku 4.5  | `claude-haiku-4-5`  | `claude-haiku-4-5-20251001`   | 200K           | 64K        | Active |

### Model Descriptions
- **Claude Fable 5** — Anthropic's most capable widely released model, for the most demanding reasoning and long-horizon agentic work. Adaptive thinking always on; GA on the Claude API, Bedrock, Google Cloud, and Microsoft Foundry since 2026-06-09. (Was temporarily suspended 2026-06-12; access restored.) **Requires 30-day data retention** — zero-data-retention (ZDR) orgs get a 400 `invalid_request_error`; use `claude-opus-4-8` (ZDR-eligible) or contact your Anthropic account team to adjust retention config.
- **Claude Mythos 5** — Invitation-only research model available through [Project Glasswing](https://anthropic.com/glasswing). Not self-serve. Contact Anthropic, AWS, or Google Cloud account team for access. **Requires 30-day data retention** — zero-data-retention (ZDR) orgs get a 400 `invalid_request_error`, even with Glasswing access; contact your Anthropic account team to adjust retention config.
- **Claude Opus 4.8** — Most capable Opus-tier model. Highly autonomous, strong on long-horizon agentic coding, complex reasoning, and high-autonomy work. Adaptive thinking; `effort` defaults to `high`. 1M context window.
- **Claude Opus 4.7** — Previous-generation Opus. Highly autonomous, strong on long-horizon agentic work. Adaptive thinking only. 1M context window — uses new tokenizer (same text ~30% more tokens vs. pre-4.7 models). See `shared/model-migration.md` → Migrating to Opus 4.7 for breaking changes.
- **Claude Opus 4.6** — Supports adaptive thinking (recommended), 128K max output tokens (requires streaming for large outputs). 1M context window.
- **Claude Sonnet 5** — Our best combination of speed and intelligence; drop-in upgrade from Sonnet 4.6 at the same price. Adaptive thinking only — manual extended thinking (`budget_tokens`) and `temperature`/`top_p`/`top_k` are rejected with a 400. New tokenizer (~30% more tokens vs. Sonnet 4.6 for the same text). `effort` defaults to `high`. 1M context window, 128K max output tokens.
- **Claude Haiku 4.5** — Fastest and most cost-effective model for simple tasks. 200K context window.

## Legacy Models (still active)

| Friendly Name     | Alias (use this)    | Full ID                       | Status |
|-------------------|---------------------|-------------------------------|--------|
| Claude Opus 4.5   | `claude-opus-4-5`   | `claude-opus-4-5-20251101`    | Active |
| Claude Opus 4.1   | `claude-opus-4-1`   | `claude-opus-4-1-20250805`    | **Deprecated** — retiring 2026-08-05 |
| Claude Sonnet 4.6 | `claude-sonnet-4-6` | —                             | Active |
| Claude Sonnet 4.5 | `claude-sonnet-4-5` | `claude-sonnet-4-5-20250929`  | Active |

> **Note:** Claude Opus 4.1 is deprecated. Migrate to `claude-opus-4-8` before 2026-08-05.

## Retired Models (no longer available)

| Friendly Name     | Full ID                       | Retired     |
|-------------------|-------------------------------|-------------|
| Claude Opus 4     | `claude-opus-4-20250514`      | Jun 15, 2026 |
| Claude Sonnet 4   | `claude-sonnet-4-20250514`    | Jun 15, 2026 |
| Claude Haiku 3    | `claude-3-haiku-20240307`     | Apr 19, 2026 |
| Claude Sonnet 3.7 | `claude-3-7-sonnet-20250219`  | Feb 19, 2026 |
| Claude Haiku 3.5  | `claude-3-5-haiku-20241022`   | Feb 19, 2026 |
| Claude Opus 3     | `claude-3-opus-20240229`      | Jan 5, 2026 |
| Claude Sonnet 3.5 | `claude-3-5-sonnet-20241022`  | Oct 28, 2025 |
| Claude Sonnet 3.5 | `claude-3-5-sonnet-20240620`  | Oct 28, 2025 |
| Claude Sonnet 3   | `claude-3-sonnet-20240229`    | Jul 21, 2025 |
| Claude 2.1        | `claude-2.1`                  | Jul 21, 2025 |
| Claude 2.0        | `claude-2.0`                  | Jul 21, 2025 |

## Resolving User Requests

When a user asks for a model by name, use this table to find the correct model ID:

| User says...                              | Use this model ID              |
|-------------------------------------------|--------------------------------|
| "most powerful", "most capable"           | `claude-fable-5` (requires 30-day data retention — ZDR orgs get a 400; use `claude-opus-4-8` under ZDR) |
| "fable", "fable 5"                        | `claude-fable-5` (requires 30-day data retention — ZDR orgs get a 400; use `claude-opus-4-8` under ZDR) |
| "mythos", "mythos 5"                      | `claude-mythos-5` (invite-only — confirm access first; also requires 30-day data retention, 400 under ZDR) |
| "opus", "opus 4.8"                        | `claude-opus-4-8`              |
| "opus 4.7"                                | `claude-opus-4-7`              |
| "opus 4.6"                                | `claude-opus-4-6`              |
| "opus 4.5"                                | `claude-opus-4-5`              |
| "opus 4.1"                                | `claude-opus-4-1` (deprecated — suggest `claude-opus-4-8`) |
| "opus 4", "opus 4.0"                      | Retired — suggest `claude-opus-4-8` |
| "sonnet", "balanced"                      | `claude-sonnet-5`              |
| "sonnet 5"                                | `claude-sonnet-5`              |
| "sonnet 4.6"                              | `claude-sonnet-4-6`            |
| "sonnet 4.5"                              | `claude-sonnet-4-5`            |
| "sonnet 4", "sonnet 4.0"                  | Retired — suggest `claude-sonnet-4-6` (Anthropic's official replacement; old call sites may still send sampling params/manual thinking/prefills that 400 on Sonnet 5 — see `shared/model-migration.md`) |
| "sonnet 3.7"                              | Retired — suggest `claude-sonnet-4-6` (see note above) |
| "sonnet 3.5"                              | Retired — suggest `claude-sonnet-4-6` (see note above) |
| "haiku", "fast", "cheap"                  | `claude-haiku-4-5`             |
| "haiku 4.5"                               | `claude-haiku-4-5`             |
| "haiku 3.5"                               | Retired — suggest `claude-haiku-4-5` |
| "haiku 3"                                 | Retired — suggest `claude-haiku-4-5` |
