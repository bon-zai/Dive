# MCP Setup for Bonzai

Bonzai uses the Model Context Protocol (MCP) to provide tools and capabilities to Zai, your AI agent.

## Configuration Location

MCP servers are configured in:
- **User Config**: `~/.bonzai/mcp_config.json` (or `$HOME/.bonzai/mcp_config.json`)

## Core MCP Tools

Bonzai comes configured with 4 core MCP tools:

### 1. Mem0 (Memory)
Provides persistent memory for Zai across conversations.

```json
{
  "mem0": {
    "command": "cmd",
    "args": ["/c", "npx", "-y", "@mem0/mcp-server"],
    "env": {
      "MEM0_API_KEY": "your_mem0_api_key",
      "DEFAULT_USER_ID": "mem0-zai-crew"
    }
  }
}
```

### 2. Desktop Commander
Enables Zai to control your desktop (file operations, system commands, etc.)

```json
{
  "desktop-commander": {
    "command": "cmd",
    "args": [
      "/c", "npx", "-y",
      "@smithery/cli@latest", "run",
      "@wonderwhy-er/desktop-commander",
      "--key", "your_smithery_key",
      "--profile", "your_smithery_profile"
    ]
  }
}
```

### 3. Brave Search
Provides web search capabilities via Brave Search API.

```json
{
  "brave-search": {
    "command": "cmd",
    "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-brave-search"],
    "env": {
      "BRAVE_API_KEY": "your_brave_api_key"
    }
  }
}
```

### 4. Chrome DevTools
Enables browser inspection and automation via Chrome DevTools Protocol.

```json
{
  "chrome-devtools": {
    "command": "cmd",
    "args": ["/c", "npx", "-y", "chrome-devtools-mcp@latest"]
  }
}
```

## Complete Example Configuration

Create or edit `~/.bonzai/mcp_config.json`:

```json
{
  "mcpServers": {
    "mem0": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@mem0/mcp-server"],
      "env": {
        "MEM0_API_KEY": "your_mem0_api_key",
        "DEFAULT_USER_ID": "mem0-zai-crew"
      }
    },
    "desktop-commander": {
      "command": "cmd",
      "args": [
        "/c", "npx", "-y",
        "@smithery/cli@latest", "run",
        "@wonderwhy-er/desktop-commander",
        "--key", "your_smithery_key",
        "--profile", "your_smithery_profile"
      ]
    },
    "brave-search": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your_brave_api_key"
      }
    },
    "chrome-devtools": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

## Platform-Specific Configuration

### Windows
Use `"command": "cmd"` and `"args": ["/c", "npx", ...]` as shown above.

### macOS/Linux
Use `"command": "npx"` directly:

```json
{
  "mem0": {
    "command": "npx",
    "args": ["-y", "@mem0/mcp-server"],
    "env": {
      "MEM0_API_KEY": "your_mem0_api_key",
      "DEFAULT_USER_ID": "mem0-zai-crew"
    }
  }
}
```

## Adding Custom MCP Servers

You can add any MCP-compatible server to Bonzai:

1. **Via GUI**:
   - Open Bonzai
   - Navigate to Settings → Tools
   - Click "Add MCP Server"
   - Fill in command, args, and environment variables

2. **Via Config File**:
   - Edit `~/.bonzai/mcp_config.json`
   - Add your server to the `mcpServers` object
   - Restart Bonzai

## Troubleshooting

### Server Won't Connect
- Check that `npx` is available in your PATH
- Verify API keys are correct
- Check logs in Bonzai Settings → System → Logs

### Missing Tools in Chat
- Ensure the server is enabled in Settings → Tools
- Restart Bonzai after configuration changes
- Check server status in Tools panel

### API Key Issues
- Mem0: Get your key at [mem0.ai](https://app.mem0.ai/)
- Brave Search: Get your key at [brave.com/search/api](https://brave.com/search/api/)
- Desktop Commander: Get credentials at [smithery.ai](https://smithery.ai/)

## Resources

- [Model Context Protocol Docs](https://modelcontextprotocol.io/)
- [MCP Server Registry](https://github.com/modelcontextprotocol/servers)
- [Smithery (Desktop Commander)](https://smithery.ai/)
- [Mem0 Documentation](https://docs.mem0.ai/)
