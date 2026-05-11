# Set up Claude MCP for GitHub with Docker Desktop

Memexis is operated by an LLM agent (Claude) that reads and writes files in your wiki repo. To give Claude that access, configure it to talk to GitHub via the [official GitHub MCP server](https://github.com/github/github-mcp-server), running locally as a Docker container. Once configured, the "Common operations" in the main [README](../README.md) (curriculum design, source ingest, project scaffold, lint pass) all work against your copy of this template.

Two Claude clients are supported:

- **Claude Desktop** (the GUI app on macOS, Windows, or Linux). Primary path for chat-based wiki work.
- **Claude Code** (the CLI tool). Secondary path for hands-on, multi-file editing during project execution.

Set up either or both. The configuration is independent.

## Prerequisites

1. **Docker Desktop** installed and running. Verify with `docker info`.
2. **A Claude client** installed: [Claude Desktop](https://claude.ai/download), or [Claude Code](https://docs.claude.com/claude-code) (`npm install -g @anthropic-ai/claude-code`), or both.
3. **A GitHub Personal Access Token (PAT)** for your fork of this repo. The `repo` scope covers private repo read and write. Add `read:org`, `workflow`, or `gist` if you'll want those toolsets. Create one at <https://github.com/settings/personal-access-tokens/new> and store it somewhere you can paste it later (a password manager, or a local `.env` file).
4. **Pull the image once** to avoid a delay on first session:

   ```
   docker pull ghcr.io/github/github-mcp-server
   ```

## Claude Desktop

Open the config file. The fastest path is inside Claude Desktop: Settings → Developer → Edit Config, which opens the file in the OS's default editor. Or open it directly:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

Add a `github` entry under `mcpServers` (create the `mcpServers` object if the file is empty or has a different shape):

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

Replace `YOUR_GITHUB_PAT` with your token. The `-e GITHUB_PERSONAL_ACCESS_TOKEN` arg with no `=value` tells Docker to pass the variable through from the parent process's environment, which is what the `env` block sets.

Save the file. Fully quit Claude Desktop (Cmd+Q on macOS; right-click the system tray icon → Exit on Windows) and reopen. Closing only the window does not reload the config.

## Claude Code

In a terminal (not inside the Claude Code REPL):

```
claude mcp add github \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_GITHUB_PAT \
  -- docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN \
  ghcr.io/github/github-mcp-server
```

To avoid putting the PAT in shell history, put it in a local `.env` (already in this template's `.gitignore`) and reference it:

```
claude mcp add github \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=$(grep GITHUB_PAT .env | cut -d '=' -f2) \
  -- docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN \
  ghcr.io/github/github-mcp-server
```

By default the config is local-scoped (current project directory only). Add `--scope user` to make the server available across all projects. Avoid `--scope project` for this repo since it writes a `.mcp.json` that gets committed; the PAT would leak on push.

## Verify

**Claude Desktop.** Open a chat, click the hammer icon in the input area, and confirm GitHub tools appear in the list (entries like `github:list_repositories`, `github:get_file_contents`, `github:create_or_update_file`). Ask Claude something like "list my repos" to confirm the PAT works end to end.

**Claude Code.** Run:

```
claude mcp list
claude mcp get github
```

In a session, run `/mcp` to see connected servers and their tool counts. Ask Claude to read a file from the repo as a smoke test.

If GitHub tools don't appear or auth fails, see Troubleshooting below.

## Optional configuration

The GitHub MCP server reads environment variables that change its behavior. Pass them as additional `-e VAR=value` args (Claude Code) or extra entries in the `env` block (Claude Desktop).

**Read-only mode.** `GITHUB_READ_ONLY=1` disables mutating tools. Useful when you want Claude to read issues, PRs, and code without being able to file or comment.

**Dynamic toolsets.** `GITHUB_DYNAMIC_TOOLSETS=1` starts with a minimal toolset and lets the model enable additional toolsets on demand. Reduces upfront tool count, which helps when many MCP servers are configured.

**Toolset selection.** `GITHUB_TOOLSETS="repos,issues,pull_requests,actions,code_security"` exposes only the listed toolsets. Trim what you don't need to reduce token overhead per turn.

**GitHub Enterprise.** `GITHUB_HOST=ghe.example.com` points the server at a GHES instance or a Cloud-with-data-residency tenant. The PAT must come from the same host.

**Remote HTTP server (Claude Code only).** Skip Docker entirely:

```
claude mcp add-json github '{"type":"http","url":"https://api.githubcopilot.com/mcp","headers":{"Authorization":"Bearer YOUR_GITHUB_PAT"}}'
```

Same toolset, no local container, PAT in the Authorization header. Claude Desktop does not yet support this because the remote server's OAuth flow requires a registered GitHub App.

**Docker MCP Toolkit.** Docker Desktop ships an MCP Toolkit (Settings → MCP Toolkit) that proxies a catalog of MCP servers through a single `MCP_DOCKER` entry. Click through Catalog → GitHub → Connect; the toolkit writes the Claude config and stores the PAT in Docker Desktop rather than the Claude config file. The tradeoff: centralized credential management against an extra proxy hop and a newer toolchain.

## Troubleshooting

**Authentication failed.** The PAT lacks the scopes a called tool needs, or has expired. Check scopes at <https://github.com/settings/tokens>. For private repo writes, `repo` is required; org-level reads need `read:org`; Actions need `workflow`.

**Docker not running.** The container starts on demand per session. If Docker Desktop is paused or hasn't fully started, the client logs a connection error and the GitHub tools fail to appear. Confirm with `docker info`.

**Image pull fails with 401.** The image is public, but `ghcr.io` sometimes returns 401 with a stale cached token. Run `docker logout ghcr.io` and retry.

**Config edits don't take effect (Desktop).** Closing the Claude Desktop window does not reload the MCP config. Fully quit (Cmd+Q on macOS, right-click system tray → Exit on Windows) and reopen.

**Env var not getting through.** The `-e GITHUB_PERSONAL_ACCESS_TOKEN` arg (no `=value`) relies on the variable being set in the spawned process's environment, which the `env` block provides. If you switch to the inline form `"-e", "GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_PAT"`, drop the `env` block. Mixing both forms is confusing and prone to silent failures.

**OAuth attempt on Claude Desktop.** Adding the remote URL `https://api.githubcopilot.com/mcp` as a custom connector on Claude Desktop fails because the remote server's OAuth flow needs a registered GitHub App. Use the Docker setup.

**Deprecated npm package.** Older guides reference `@modelcontextprotocol/server-github` on npm. That package was deprecated in April 2025. The supported server is `ghcr.io/github/github-mcp-server` (Docker) or the binary from the [releases page](https://github.com/github/github-mcp-server/releases).

**Reading logs.** When tools don't appear after restart, check the server logs:

- macOS (Desktop): `cat ~/Library/Logs/Claude/mcp-server-github.log`
- Windows (Desktop): `%APPDATA%\Claude\logs\`
- Claude Code: `/mcp` inside the REPL

## References

- [GitHub MCP server install guide for Claude](https://github.com/github/github-mcp-server/blob/main/docs/installation-guides/install-claude.md)
- [GitHub MCP server repository](https://github.com/github/github-mcp-server)
