# Novamira WordPress MCP Remote On Windows

Use this reference when setting up, fixing, or verifying a Novamira WordPress MCP server through `@automattic/mcp-wordpress-remote`, especially on Windows 10/11 clients.

## Correct Config Shape

For `@automattic/mcp-wordpress-remote`, pass WordPress connection details only as environment variables. Do not use CLI flags such as `--url`, `--username`, or `--password`; the package may ignore them.

Use this command shape:

```json
{
  "command": "npx",
  "args": ["-y", "@automattic/mcp-wordpress-remote@latest"],
  "env": {
    "WP_API_URL": "https://example.com/wp-json/mcp/server",
    "WP_API_USERNAME": "username",
    "WP_API_PASSWORD": "application-password"
  }
}
```

On this Windows Codex client, Novamira WordPress remotes have repeatedly needed these extra environment values when using application passwords:

```json
{
  "env": {
    "NODE_OPTIONS": "--use-system-ca",
    "OAUTH_ENABLED": "false"
  }
}
```

Use `NODE_OPTIONS = "--use-system-ca"` when Node TLS verification fails or when comparable Novamira servers in the same client already use it. Use `OAUTH_ENABLED = "false"` when the project is explicitly configured for WordPress application passwords; otherwise the proxy may try OAuth first and waste time or open an unwanted auth path. These are environment values, not CLI flags.

Keep credentials out of skills, Markdown notes, and project docs. Store them only in the MCP client config environment values.

## Windows-Specific Failure Points

- The AI client may not inherit the same `PATH` as an interactive PowerShell terminal. `npx` can work in PowerShell but fail inside the client.
- If `npx` is not found by the client, prefer using the absolute Windows path to `npx.cmd` in config, after verifying it with `where.exe npx`.
- Some clients need a full restart, not just a UI reload, before new MCP server config is picked up.
- If the config changed and the tool list did not, restart the MCP session/client before debugging the WordPress server.
- Corporate security tools or PowerShell execution policy can block helper scripts, but `npx` itself does not require a PowerShell script wrapper when the client can launch it correctly.
- The Automattic proxy uses the MCP SDK `StdioServerTransport`, which expects newline-delimited JSON messages on stdio. Do not test it with `Content-Length` framed messages; that produces silent timeouts with no useful stderr.
- Directly spawning `npx.cmd` from a Node verification script may fail on Windows with `spawn EINVAL` even when `npx` works in PowerShell and the Codex client can launch it. For manual diagnostics, either spawn `cmd.exe /c npx.cmd -y @automattic/mcp-wordpress-remote@latest` or run the cached `dist/proxy.js` through `node`.

## Verification Workflow

After writing or changing MCP config:

1. Restart/reload the MCP session or client.
2. List available MCP tools/servers in the active session.
3. Confirm the expected Novamira server name appears.
4. Run a harmless read-only ability such as discovering abilities or listing memories/tools.
5. If startup fails, inspect the `npx` process stderr before proposing config changes.

If the server is not visible in the active session yet, verify the remote without changing credentials:

1. Check the endpoint with a simple unauthenticated HTTPS GET. A `401` JSON response from `/wp-json/mcp/...` is useful evidence: the route is reachable and not replaced by a maintenance page, login redirect, WAF block, or HTML error page.
2. Run a local stdio MCP handshake using newline-delimited JSON and call `tools/list`.
3. If `tools/list` succeeds, call `mcp-adapter-discover-abilities` or another harmless read-only ability.
4. Treat a successful direct `tools/list` plus ability call as proof that the WordPress endpoint, credentials, and proxy package work; the remaining issue is then client reload/config loading.

Do not guess at credentials or rewrite the server URL if the only evidence is a missing tool list. First distinguish:

- config not loaded,
- `npx`/Node launch failure,
- remote package startup failure,
- WordPress authentication failure,
- Novamira endpoint/server-side failure.

## Troubleshooting Order

1. Confirm the MCP config server name matches the user's requested name.
2. Confirm `args` are exactly `["-y", "@automattic/mcp-wordpress-remote@latest"]` unless the user explicitly chose another package/version.
3. Confirm `WP_API_URL`, `WP_API_USERNAME`, and `WP_API_PASSWORD` are present in `env`.
4. Compare the env block with other working Novamira servers in `C:\Users\Anwender\.codex\config.toml`; if they use `NODE_OPTIONS = "--use-system-ca"` and `OAUTH_ENABLED = "false"`, add the same values to the new server before spending time on deeper TLS/OAuth debugging.
5. Confirm the client can launch `npx`; on Windows, use `where.exe npx` or an absolute `npx.cmd` path if needed.
6. Restart the client/session and list tools again.
7. If it still fails, show the `npx` stderr to the user before suggesting changes.

## Manual Stdio Smoke Test

When the Codex session has not loaded the new server yet, use this shape for a local smoke test. Send one JSON object per line, not `Content-Length` frames:

```text
{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"codex-verify","version":"1.0.0"}}}
{"jsonrpc":"2.0","method":"notifications/initialized","params":{}}
{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}
```

Expected success for a Novamira WordPress server is a tool list containing:

- `mcp-adapter-discover-abilities`
- `mcp-adapter-get-ability-info`
- `mcp-adapter-execute-ability`

Then verify the WordPress side with a read-only call:

```json
{
  "jsonrpc": "2.0",
  "id": 3,
  "method": "tools/call",
  "params": {
    "name": "mcp-adapter-discover-abilities",
    "arguments": {}
  }
}
```

If this direct test succeeds but the active Codex session does not show the new namespace, do not keep editing the WordPress credentials. Restart/reload the client again and re-check the available MCP tools.

## User-Facing Fallback

If the current AI client cannot modify its MCP config from the active environment, tell the user to open the Novamira Configuration page, expand "Need the JSON config for a specific client?", and copy the client-specific JSON snippet manually.
