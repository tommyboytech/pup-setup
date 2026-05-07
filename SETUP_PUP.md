# Setting Up Pup CLI for AI Coding Agents

This document provides instructions for an AI coding agent to set up the [Pup CLI](https://github.com/datadog-labs/pup) and install all available skills.

## Overview

Pup is a comprehensive CLI for Datadog's observability platform, with 325+ commands across 57 product domains. It includes 9 skills and 48 domain agents that can be installed into your AI coding assistant (Claude Code, Cursor, etc.).

## Quick Start (for AI Agents)

As an AI coding agent, follow the manual steps below, using your available tool calls (bash, read, write, edit, etc.) to execute commands and modify files. Each step includes prompts for user preferences where needed.

## Prerequisites

- A Unix-like environment (macOS, Linux, WSL)
- Homebrew (recommended) or Rust toolchain for building from source
- A Datadog account with API access (OAuth2 or API key authentication)

## Installation Steps

### 1. Install Pup CLI

**Option A: Homebrew (macOS/Linux) - Recommended**
```bash
brew tap datadog-labs/pack
brew install datadog-labs/pack/pup
```

**Option B: Manual Binary Download**
```bash
# Download the latest release for your platform
curl -L https://github.com/datadog-labs/pup/releases/latest/download/pup-$(uname -s | tr '[:upper:]' '[:lower:]')-$(uname -m) -o pup
chmod +x pup
sudo mv pup /usr/local/bin/
```

**Option C: Build from Source (requires Rust)**
```bash
git clone https://github.com/datadog-labs/pup.git
cd pup
cargo build --release
cp target/release/pup /usr/local/bin/
```

**Option D: Claude Code Plugin (if using Claude Code)**
Pup is available as a Claude Code plugin marketplace:
```bash
/plugin marketplace add datadog-labs/pup
```
This installs the pup plugin but not the CLI. You'll still need to install the CLI separately via Option A, B, or C.

### 2. Verify Installation
```bash
pup --version
```

### 3. Authenticate with Datadog

**Option A: OAuth2 (Recommended)**
```bash
# Set your Datadog site (optional, defaults to datadoghq.com)
export DD_SITE="datadoghq.com"

# Login via browser
pup auth login

# Verify authentication
pup auth status
```

**Option B: API Key Authentication**
```bash
export DD_API_KEY="your-datadog-api-key"
export DD_APP_KEY="your-datadog-application-key"
export DD_SITE="datadogq.com"  # Optional, defaults to datadoghq.com
```

**Option C: Bearer Token**
```bash
export DD_ACCESS_TOKEN="your-oauth-access-token"
export DD_SITE="datadoghq.com"
```

### 4. Test Connection
```bash
pup test
```

## Skills Installation

Pup includes 9 skills and 48 domain agents that can be installed to your AI assistant's configuration.

### Choose Installation Location

First, attempt to detect which AI coding agent is in use using multiple methods (environment variables may not be set):

1. **Check environment variables** (if present):
   - `CLAUDE_CODE` or `CLAUDECODE` → Claude Code
   - `CURSOR_AGENT` → Cursor
   - `CODEX` or `OPENAI_CODEX` → OpenAI Codex
   - `AIDER` → Aider
   - `CLINE` → Cline
   - `WINDSURF_AGENT` → Windsurf
   - `GITHUB_COPILOT` → GitHub Copilot
   - `AMAZON_Q` or `AWS_Q_DEVELOPER` → Amazon Q Developer
   - `GEMINI_CODE_ASSIST` → Gemini Code Assist
   - `SRC_CODY` → Sourcegraph Cody

2. **Check for agent-specific directories** in the current project or home directory:
   - `.claude/` → Claude Code
   - `.gemini/` → Gemini Code Assist
   - `.copilot/` or `.github/copilot/` → GitHub Copilot
   - `.codex/` → OpenAI Codex
   - `.opencode/` or `~/.config/opencode/` → opencode

3. **If detection is inconclusive**, explicitly ask the user which agent they are using, providing a list of supported options:
   - Claude Code
   - Gemini Code Assist
   - OpenAI Codex
   - GitHub Copilot
   - opencode
   - Other (specify)

**Where should skills be installed?** Present options with examples relevant to the detected agent:

- **Global**: Install to the agent's global config directory. Examples:
  - Claude Code: `~/.claude/skills/`
  - Gemini Code Assist: `~/.gemini/skills/`
  - OpenAI Codex: `~/.codex/skills/`
  - GitHub Copilot: `~/.copilot/skills/`
  - opencode: `~/.config/opencode/skills/`
  - Other agents: `~/.config/<agent>/skills/` or `~/.local/share/<agent>/skills/`
- **Project-specific**: Install to the current project's config directory. Examples:
  - Claude Code: `.claude/skills/`
  - Gemini Code Assist: `.gemini/skills/`
  - OpenAI Codex: `.codex/skills/`
  - GitHub Copilot: `.copilot/skills/`
  - opencode: `.opencode/skills/`
  - Other agents: `.<agent>/skills/` or `.pup/skills/`
- **Custom**: Specify any absolute or relative directory path

### Install Skills

`pup skills install` chooses the install location based on the current working directory: if it finds an agent-specific directory (e.g. `.claude/`) by walking up from cwd, it installs there; otherwise it falls back to `$HOME`. Run `pup skills path` first to confirm where files will be written.

- **Global installation** — run from a directory without a project agent dir (e.g. `$HOME`):
  ```bash
  cd ~ && pup skills install
  ```
  For Claude Code this writes skills to `~/.claude/skills/<skill>/` and agents to `~/.claude/agents/<agent>.md`.

- **Project-specific installation** — run from inside the project:
  ```bash
  cd /path/to/project && pup skills install
  ```
  This writes to `<project>/.claude/skills/` and `<project>/.claude/agents/`.

- **Custom installation** — override the install dir with `--dir`:
  ```bash
  pup skills install --dir=/path/to/install/root
  ```
  Note: `--dir` is the literal install root for everything. Pointing it at a leaf directory like `~/.claude` flattens skills and agents into `~/.claude/<name>/SKILL.md` instead of preserving the `skills/` + `agents/` layout. For Claude Code's normal layout, prefer the `cd` approach above.

You can also install specific skills individually by appending the skill name (e.g., `pup skills install dd-monitors`).

#### List Available Skills:
```bash
pup skills list
pup skills list --type=skill      # Skills only
pup skills list --type=agent      # Agents only
```

## Agent Mode Configuration

When Pup is invoked by an AI coding agent, it automatically switches to agent mode for optimized JSON responses. Ensure your environment variable is set:

```bash
# For Claude Code (auto-detected)
export CLAUDE_CODE=1

# For other supported agents
export GEMINI_CODE_ASSIST=1   # Gemini Code Assist
export CODEX=1                # OpenAI Codex
export GITHUB_COPILOT=1       # GitHub Copilot
export FORCE_AGENT_MODE=1     # opencode or any other agent

# You can also set these variables for other agents if needed
export CURSOR_AGENT=1         # Cursor
export AIDER=1                # Aider
```

## Verification

After installation, verify everything works:

1. **Check Pup version**:
   ```bash
   pup --version
   ```

2. **Check authentication**:
   ```bash
   pup auth status
   ```

3. **List installed skills**:
   ```bash
   pup skills list
   ```

4. **Test a command**:
   ```bash
   pup monitors list
   ```

## Available Skills

| Skill | Description |
|-------|-------------|
| `dd-pup` | Primary CLI - all pup commands, auth, site config |
| `dd-monitors` | Create, manage, mute monitors and alerts |
| `dd-logs` | Search logs, pipelines, archives |
| `dd-apm` | Traces, services, performance analysis |
| `dd-docs` | Search Datadog documentation via llms.txt |
| `dd-code-generation` | CLI vs code-gen decision, multi-language examples |
| `dd-file-issue` | Issue routing to correct repo, duplicate search |
| `dd-debugger` | Live Debugger — inspect runtime values via log probes |
| `dd-symdb` | Symbol Database — search service symbols, find probe-able methods |

## Available Domain Agents (48)

Specialized agents for every Datadog API domain including:
- Infrastructure & Cloud (AWS, GCP, Azure, OCI)
- Monitoring & Alerting (Monitors, Dashboards, SLOs)
- Logs, Metrics, APM, RUM
- Security (Security Monitoring, Audit Logs, SCA)
- Incident Management & Case Management
- CI/CD, Workflows, Runbooks
- And many more...

## Troubleshooting

### Common Issues

1. **Pup not found in PATH**:
   - Ensure `/usr/local/bin` is in your `PATH`
   - Try reinstalling with `brew reinstall pup`

2. **Authentication failures**:
   - Verify your API keys are correct
   - Check your Datadog site configuration
   - For OAuth2, ensure DCR is enabled on your Datadog site

3. **Skills installation fails**:
   - Ensure you have write permissions to the target directory
   - Try with `--verbose` flag: `pup skills install --verbose`

## Additional Resources

- [Pup GitHub Repository](https://github.com/datadog-labs/pup)
- [Pup Documentation](https://github.com/datadog-labs/pup/blob/main/docs/COMMANDS.md)
- [OAuth2 Setup Guide](https://github.com/datadog-labs/pup/blob/main/docs/OAUTH2.md)
- [Examples](https://github.com/datadog-labs/pup/blob/main/docs/EXAMPLES.md)

## Notes for AI Coding Agents

When following these instructions, as an AI coding agent you should:

1. **Detect which AI agent is in use** using the multi‑method detection described in "Choose Installation Location". Environment variables may not be set, so also check for agent‑specific directories and, if needed, ask the user directly.
2. **Explicitly support** the following agents: Claude Code, Gemini Code Assist, OpenAI Codex, GitHub Copilot, and opencode.
3. **Prompt the user** about their preferred installation location (global, project‑specific, or custom). Provide examples relevant to the detected agent.
4. **Handle authentication** by prompting for credentials if environment variables aren't set. Check `pup auth status` first.
5. **Use your tool calls** to execute bash commands, read/write files, and edit configuration as needed.
6. **Verify success** by running a test command after each major step (e.g., `pup --version`, `pup auth status`, `pup skills list`).
7. **Report completion** with a summary of what was installed, including the number of skills and agents.

