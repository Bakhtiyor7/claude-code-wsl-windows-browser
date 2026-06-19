Here's a cleaned-up version with clearer structure and wording:

---

# Use Windows Chrome with Playwright MCP in Claude Code on WSL

## Background

When Claude Code runs inside WSL, Playwright MCP tries to launch a browser within the Linux environment. Since most Windows users keep their browsers installed on the Windows side (not inside WSL), Playwright can't find one — or it launches an invisible browser that you can't see or interact with.

The fix is to run the Playwright MCP server through `cmd.exe` so it runs on the Windows side, where Chrome is already installed.

## Configuration

In `~/.claude.json`, find your project under the `"projects"` key and add the following to its `"mcpServers"` block:

```json
{
  "mcpServers": {
    "playwright": {
      "type": "stdio",
      "command": "cmd.exe",
      "args": [
        "/c",
        "npx",
        "@playwright/mcp@latest",
        "--browser",
        "chrome"
      ]
    }
  }
}
```

## How to Apply

**Option 1 — Through Claude Code:**
1. Open Claude Code inside your project.
2. Run `/mcp`.
3. Select **Add server**.
4. Choose the manual or JSON configuration option.
5. Paste the configuration above.

**Option 2 — Edit the file directly:**

Open `~/.claude.json`, find your project inside the `"projects"` section, then add or update its `"mcpServers"` block with the configuration above.

## Why This Works

By default, Claude Code runs `npx` inside WSL. If Chrome isn't installed there, Playwright fails to launch.

Invoking `cmd.exe /c npx ...` instead runs the Playwright MCP server on the Windows host, giving it access to the Chrome installation on that side.

## Requirements

- Google Chrome must be installed on Windows.
- Node.js and `npx` must be available on Windows (not just inside WSL).
- The first run of `npx @playwright/mcp@latest` may prompt you to install the package.

---
