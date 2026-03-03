# Quorum

Quorum is a Rust-first coding agent CLI with first-class multi-provider support.

It supports OpenAI/Codex, Anthropic, Grok/xAI, Gemini, Azure OpenAI, and more, while still preserving provider-specific strengths instead of flattening everything into a lowest-common-denominator abstraction.

Public distribution:

- Releases: https://github.com/xudesheng/quorum/releases
- Latest release: https://github.com/xudesheng/quorum/releases/latest
- Install guide: [install.md](./install.md)
- Config reference: [config.md](./config.md)

## Quick Install

Fastest path:

```bash
npm i -g @xudesheng/quorum
quorum --version
```

For all install methods (npm, GitHub binaries, Homebrew, winget, bunx), see [install.md](./install.md).

## Quick Start

Recommended config path:

- `~/.quorum/config.toml`

Full config reference: [config.md](./config.md)

Example config (OpenAI + Anthropic + Grok + Gemini + Azure OpenAI + Codex OAuth):

```toml
[defaults]
config_item = "openai-prod"

[[config_items]]
id = "openai-prod"
provider = "openai_responses"
description = "OpenAI with inline secret example"

[config_items.auth]
mode = "api_key"
secret = "sk-proj-REPLACE_ME"

[config_items.model_policy]
default_model = "gpt-5.3-codex"
allowed_models = ["gpt-5.3-codex"]
mutable = false

[[config_items]]
id = "anthropic-prod"
provider = "anthropic"

[config_items.auth]
mode = "api_key"
env_var = "ANTHROPIC_API_KEY"

[config_items.model_policy]
default_model = "claude-opus-4-6"
allowed_models = ["claude-opus-4-6"]
mutable = false

[[config_items]]
id = "grok-prod"
provider = "grok"

[config_items.auth]
mode = "api_key"
env_var = "XAI_API_KEY"

[config_items.model_policy]
default_model = "grok-4-1-fast-reasoning"
allowed_models = ["grok-4-1-fast-reasoning"]
mutable = false

[[config_items]]
id = "gemini-prod"
provider = "gemini"

[config_items.auth]
mode = "api_key"
env_var = "GEMINI_API_KEY"

[config_items.model_policy]
default_model = "gemini-3-flash-preview"
allowed_models = ["gemini-3-flash-preview"]
mutable = false

[[config_items]]
id = "azure-openai-prod"
provider = "azure_openai"

[config_items.auth]
mode = "api_key"
env_var = "AZURE_OPENAI_API_KEY"

[config_items.endpoint]
type = "custom"
base_url = "https://YOUR_RESOURCE.openai.azure.com/openai/v1"
deployment = "gpt-5-3-codex-deploy"
api_version = "2025-04-01-preview"

[config_items.model_policy]
default_model = "gpt-5.3-codex"
allowed_models = ["gpt-5.3-codex"]
mutable = false

[[config_items]]
id = "codex-oauth-dev"
provider = "openai_responses"

[config_items.auth]
mode = "oauth"

[config_items.model_policy]
default_model = "gpt-5.3-codex"
allowed_models = ["gpt-5.3-codex"]
mutable = false
```

Notes:

- Anthropic OAuth is currently restricted by Anthropic. Use `api_key` for Anthropic.
- Gemini uses API key auth only in Quorum; configure `GEMINI_API_KEY`.
- For API key auth, configure either `secret` or `env_var`; only one is required.
- `defaults.config_item` is the convenience entry point for daily use.

## Supported Features

### 1) MCP Server

Quorum supports MCP servers over `stdio` and `streamable-http`.

Example (`.quorum/mcp.toml`):

```toml
[mcp-servers.mcp_test]
transport = "stdio"
command = "/Users/you/.local/bin/uv"
args = ["run", "/path/to/minimal_server.py"]
enabled = true
allow_indirect_launcher = true
```

### 2) Skills

Quorum supports built-in starter skills and custom `SKILL.md` skills.

Skill locations:

- User scope: `~/.quorum/skills/<skill-name>/SKILL.md`
- Project scope: `<workspace>/.quorum/skills/<skill-name>/SKILL.md`

Minimal `SKILL.md`:

```md
---
name: commit_helper
version: 1.0.0
trigger:
  slash: /commit-helper
---

## setup
You help draft concise conventional commit messages.
```

### 3) Auto-Learn

Quick docs:

- English: [auto-learn.md](./auto-learn.md)
- 中文: [auto-learn_CN.md](./auto-learn_CN.md)

Samples:

- [samples/AUTO-LEARNED.md](./samples/AUTO-LEARNED.md) (sample index)
- [samples/learned/debugging.md](./samples/learned/debugging.md) (sample topic)

Common setup:

- `~/.quorum/AUTO-LEARNED.md`
- `~/.quorum/learned/*.md`

### 4) Steering Message

Quorum supports runtime steering/follow-up while a turn is running:

- `Enter`: steer
- `Tab`: follow-up
- `Esc`: interrupt

This is useful for correcting direction during long responses or tool execution.
