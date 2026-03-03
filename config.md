# Quorum `config.toml` Reference

This page documents the supported `config.toml` schema for Quorum public usage.

Recommended file path:

- `~/.quorum/config.toml`

## Full example

```toml
[defaults]
config_item = "openai-prod"
profile = "dev"

[runtime]
allow_insecure_localhost = false

[[config_items]]
id = "openai-prod"
provider = "openai_responses"
description = "Primary OpenAI profile"

[config_items.auth]
mode = "api_key"
env_var = "OPENAI_API_KEY"

[config_items.model_policy]
default_model = "gpt-5.3-codex"
allowed_models = ["gpt-5.3-codex"]
mutable = false

[[config_items]]
id = "azure-openai-prod"
provider = "azure_openai"

[config_items.auth]
mode = "api_key"
env_var = "AZURE_OPENAI_API_KEY"

[config_items.endpoint]
base_url = "https://YOUR_RESOURCE.openai.azure.com/openai/v1"
deployment = "gpt-5-3-codex-deploy"
api_version = "2025-04-01-preview"

[config_items.model_policy]
default_model = "gpt-5.3-codex"
allowed_models = ["gpt-5.3-codex"]
mutable = false

[[config_items]]
id = "gemini-prod"
provider = "gemini"
description = "Gemini Tier-1 profile"

[config_items.auth]
mode = "api_key"
env_var = "GEMINI_API_KEY"

[config_items.model_policy]
default_model = "gemini-3-flash-preview"
allowed_models = ["gemini-3-flash-preview"]
mutable = false

[logging]
level = "info"
audit_buffer_size = 4096
max_field_depth = 4

[logging.components]
"quorum::provider" = "debug"

[logging.sampling]
"quorum::usage::turn" = 1.0

[logging.rate_limit]
"quorum::hooks" = 20

[logging.events."quorum::config"]
include = ["config_item_resolved"]

[compaction]
tail_window = 6
trigger_threshold = 20
max_summary_tokens = 1024
strategy = "Extractive"
summary_injection_role = "User"
validation_mode = "Lenient"
context_utilization_trigger = 0.80
enable_model_summary = false
model_summariser_timeout_ms = 30000
provider_token_counter_timeout_ms = 5000
```

## Top-level sections

### `[defaults]`

- `config_item` (string, optional): default config item ID.
- `profile` (string, optional): default profile name.

### `[runtime]`

- `allow_insecure_localhost` (bool, optional, default: `false`):
  allows `http://localhost`, `http://127.0.0.1`, `http://[::1]` custom endpoints.

### `[[config_items]]`

Defines one provider/auth/model target. At least one item is required.

- `id` (string, required):
  - regex: `^[a-z0-9][a-z0-9_-]{0,63}$`
  - must be unique in the catalog
  - do not start with `legacy-` or `legacy_`
- `provider` (string, required):
  - valid: `anthropic`, `openai_responses`, `openai_chat`, `azure_openai`, `grok`, `gemini`
  - alias: `openai` (mapped to `openai_responses`)
- `description` (string, optional)

#### `[config_items.auth]`

- `mode` (string, required): `api_key` or `oauth`
- `env_var` (string, optional): environment variable name for API key
- `secret` (string, optional): inline API key

For API key auth, use either `secret` or `env_var` (one is enough).

Auth compatibility matrix:

- `anthropic`: `api_key` only
- `openai_responses`: `api_key` or `oauth`
- `openai_chat`: `api_key` or `oauth`
- `azure_openai`: `api_key` only
- `grok`: `api_key` only
- `gemini`: `api_key` only (`GEMINI_API_KEY`)

#### `[config_items.endpoint]`

Optional custom endpoint.

- `base_url` (string, optional)
- `deployment` (string, optional; required for `azure_openai`)
- `api_version` (string, optional)

Rules:

- If `base_url` is omitted, provider default endpoint is used.
- For `azure_openai`, custom endpoint is required and must include both `base_url` and `deployment`.
- `https://` is required by default.
- `http://localhost` is allowed only when `runtime.allow_insecure_localhost = true`.

#### `[config_items.model_policy]`

- `default_model` (string, required)
- `allowed_models` (string array, optional; default `[]`)
- `mutable` (bool, optional; default `false`)

Behavior:

- If `mutable = false`, runtime model overrides are ignored and `default_model` is used.
- If `allowed_models` is non-empty and does not include `default_model`, Quorum auto-inserts `default_model`.

### `[logging]`

All logging fields are optional.

- `level` (string): `trace|debug|info|warn|error` (default: `info`)
- `audit_buffer_size` (int): default `4096`, clamped to `[256, 65536]`
- `max_field_depth` (int): default `4`, clamped to `[1, 8]`

Optional maps:

- `[logging.components]`: per-component log level override
- `[logging.sampling]`: per-component sample rate (`0.0..1.0`)
- `[logging.rate_limit]`: per-component events/sec (`0` means unlimited)
- `[logging.events."<component>"]`:
  - `include = ["event_a", ...]` or `exclude = ["event_b", ...]`
  - `include` and `exclude` are mutually exclusive

### `[compaction]`

Optional memory compaction configuration.

- `tail_window` (int, default: `6`)
- `trigger_threshold` (int, default: `20`)
- `max_summary_tokens` (int, default: `1024`)
- `strategy` (string, default: `Extractive`):
  `Extractive|ModelSummary|Hybrid`
- `summary_injection_role` (string, default: `User`): `User|System`
- `validation_mode` (string, default: `Lenient`): `Lenient|Strict`
- `context_utilization_trigger` (float, default: `0.80`)
- `enable_model_summary` (bool, default: `false`)
- `model_summariser_timeout_ms` (int, default: `30000`)
- `provider_token_counter_timeout_ms` (int, default: `5000`)

## Profile and project override files

Profile and project layering is supported:

- Profile file: `~/.quorum/profiles/<name>.toml`
- Project file: `.quorum/config.toml`

Selection precedence for profile name:

1. `--profile`
2. `QUORUM_PROFILE`
3. `[defaults].profile`

Merge behavior:

- `config_items` are merged by `id` (same `id` in profile/project replaces base entry).
- Top-level sections like `defaults`, `runtime`, and `logging` are override-wins.
