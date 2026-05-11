---
type: resource
category: mcp
status: polished
created: 2026-05-11
updated: 2026-05-11
applies_to:
  - claude-desktop
  - claude-code
  - github
related: []
sources:
  - title: Install GitHub MCP Server in Claude Applications
    url: https://github.com/github/github-mcp-server/blob/main/docs/installation-guides/install-claude.md
  - title: GitHub MCP Server (official repository)
    url: https://github.com/github/github-mcp-server
---

# GitHub MCP server with Docker Desktop

## What this is

Setup for running [GitHub's official MCP server](https://github.com/github/github-mcp-server) as a local Docker container, exposed to Claude Desktop or Claude Code via stdio. Claude reads and writes GitHub repos, issues, and pull requests through the MCP tool interface, authenticated with a Personal Access Token.

The container image is `ghcr.io/github/github-mcp-server`. Docker Desktop provides the runtime. The Claude client launches the container per session and tears it down on exit (`--rm`).

## When to apply

Use this setup when:

- You want Claude (Desktop or Code) to operate on private repos with PAT-scoped access rather than OAuth.
- You're using Claude Desktop, which currently does not support the remote GitHub MCP server because the remote endpoint requires OAuth through a registered GitHub App. Local Docker is the only supported Desktop path.
- You want the GitHub MCP server alongside other local stdio MCP servers in the same `mcpServers` config block.

## When not to apply

Skip this setup when:

- You're on Claude Code and prefer the remote HTTP server (`https://api.githubcopilot.com/mcp`) with a PAT in the Authorization header. The remote server has no local container, the same toolset, and works fine for Claude Code. See variations below.
- You're using a claude.ai connector for GitHub. That's a separate integration and doesn't run locally.
- Docker isn't available on the machine (locked-down workstation, no admin rights). Use the prebuilt binary from the GitHub MCP server's releases page instead.

## How it works

The Claude client spawns the Docker container as a child process and wires its stdin/stdout to the MCP protocol. Each session starts a fresh container; the `--rm` flag removes it on exit. The PAT is passed in via environment variable; the server uses it for every GitHub API call.

### Prerequisites

1. Docker Desktop installed and running. Confirm with `docker info`.
2. A GitHub Personal Access Token with the scopes the server needs. The `repo` scope covers private repo read and write. Add `read:org`, `workflow`, `gist`, etc. if you need those toolsets. Create one at <https://github.com/settings/personal-access-tokens/new>.
3. Optional but speeds up first launch: pull the image once.

   ```
   docker pull ghcr.io/github/github-mcp-server
   ```

### Claude Desktop

Edit the Claude Desktop config file:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

The fastest path is Settings → Developer → Edit Config inside Claude Desktop, which opens the file in the OS's default editor.

Add a `github` entry under `mcpServers`:

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_PAT"
      }
    }
  }
}
```

The `-e GITHUB_PERSONAL_ACCESS_TOKEN` arg with no `=value` tells Docker to pass the variable through from the parent process's environment, which is what the `env` block sets. Replace `YOUR_GITHUB_PAT` with the actual token.

Fully quit Claude Desktop (Cmd+Q on macOS, right-click the system tray icon → Exit on Windows) and reopen. Closing only the window does not reload the config. After restart, the GitHub tools appear in the tool picker (hammer icon in the input area).

### Claude Code

Run this in a terminal (not inside the Claude Code REPL):

```
claude mcp add github \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_GITHUB_PAT \
  -- docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN \
  ghcr.io/github/github-mcp-server
```

To source the PAT from a `.env` file rather than hardcoding it on the command line:

```
claude mcp add github \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=$(grep GITHUB_PAT .env | cut -d '=' -f2) \
  -- docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN \
  ghcr.io/github/github-mcp-server
```

Verify:

```
claude mcp list
claude mcp get github
```

By default the config is local-scoped (only the current project directory). Add `--scope user` to make the server available across all projects. Add `--scope project` to share via a committed `.mcp.json` in the repo (in which case never hardcode the PAT; see pitfalls).

## Example

A `claude_desktop_config.json` with the GitHub MCP server alongside another stdio server (the official filesystem server):

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxx"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/you/projects"
      ]
    }
  }
}
```

Both servers load on Claude Desktop startup and their tools merge into one picker.

## Variations and extensions

**Read-only mode.** Add `GITHUB_READ_ONLY=1` to the env block to disable mutating tools. Useful when you want Claude to read issues, PRs, and code without being able to file or comment.

```json
"env": {
  "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_PAT",
  "GITHUB_READ_ONLY": "1"
}
```

**Dynamic toolsets.** Add `GITHUB_DYNAMIC_TOOLSETS=1` to start with a minimal toolset and let the model enable additional toolsets on demand. Reduces upfront tool count, which helps the model when many MCP servers are configured.

**Toolset selection.** Add `GITHUB_TOOLSETS="repos,issues,pull_requests,actions,code_security"` to expose only listed toolsets. Trim what you don't need to reduce token overhead per turn.

**Remote HTTP server (Claude Code only).** Skip Docker entirely:

```
claude mcp add-json github '{"type":"http","url":"https://api.githubcopilot.com/mcp","headers":{"Authorization":"Bearer YOUR_GITHUB_PAT"}}'
```

Same toolset, no local container, PAT in the Authorization header. Claude Desktop does not yet support this because the remote server's OAuth flow requires a registered GitHub App. Stay with Docker for Desktop.

**Docker MCP Toolkit.** Docker Desktop ships an MCP Toolkit (Settings → MCP Toolkit) that proxies a catalog of MCP servers through a single `MCP_DOCKER` entry written into the Claude config. Click through Catalog → GitHub → Connect; the toolkit writes the config and stores the PAT in Docker Desktop rather than the Claude config. The tradeoff: centralized credential storage and easy multi-server management, against an extra proxy hop (alpine/socat → host.docker.internal:8811 → server container) and a newer toolchain that's less battle-tested than the direct setup.

**GitHub Enterprise.** Set `GITHUB_HOST` in the env block (or pass `--gh-host` to the binary) to point at a GHES instance or a Cloud-with-data-residency tenant. The PAT must be from the same host.

## Common pitfalls

**Authentication failed.** The PAT lacks the scopes the called tool needs, or has expired. Check scopes at <https://github.com/settings/tokens>. For private repo write access, `repo` is required; for org-level reads, add `read:org`; for Actions, add `workflow`.

**Docker not running.** The container starts on demand per session. If Docker Desktop is paused or hasn't started yet, the client logs a connection error and the GitHub tools fail to appear. Confirm with `docker info`.

**Image pull fails with 401.** The image is public, but `ghcr.io` sometimes returns 401 if you have a stale token cached for the registry. Run `docker logout ghcr.io` and retry the pull.

**Config edits don't take effect.** Closing the Claude Desktop window does not reload the MCP config. Fully quit (Cmd+Q on macOS, right-click system tray → Exit on Windows) and reopen.

**Env var not getting through.** The `-e GITHUB_PERSONAL_ACCESS_TOKEN` arg with no value relies on the variable being set in the spawned process's environment, which the `env` block provides. If you switch to the inline form `"-e", "GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_PAT"`, drop the `env` block. Mixing both forms is confusing and prone to silent failures where the token isn't where you think it is.

**Hardcoded PAT in committed config.** Project-scoped Claude Code config lives in `.mcp.json` in the repo. Hardcoding the PAT there leaks it on push. Either source the PAT from a local `.env` and reference it via shell expansion at `claude mcp add` time, or add `.mcp.json` to `.gitignore` and document the setup separately in the repo's README.

**OAuth attempt on Claude Desktop.** Adding the remote URL `https://api.githubcopilot.com/mcp` as a custom connector on Claude Desktop fails because the remote server's OAuth flow needs a registered GitHub App, which Desktop does not provide. Stick with the Docker setup.

**Using the deprecated npm package.** Older guides reference `@modelcontextprotocol/server-github` on npm. That package was deprecated in April 2025. The supported server is `ghcr.io/github/github-mcp-server` (Docker) or the binary from the releases page.

**Reading logs.** When tools don't appear after restart, check the server logs:

- macOS: `ls ~/Library/Logs/Claude/` then `cat ~/Library/Logs/Claude/mcp-server-github.log`
- Windows: `%APPDATA%\Claude\logs\`
- Claude Code: `/mcp` command inside the REPL

## Origin

Needed for using Claude Desktop and Claude Code to interact with personal and Memexis-wiki GitHub repos. Captured as a resource rather than a project log entry because the setup applies across every GitHub-touching project.

## See also

<!-- agent-maintained from related: -->

No related entries yet. Future MCP server setup notes will live alongside this one in `resources/mcp/`.
