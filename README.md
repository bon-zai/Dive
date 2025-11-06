<div align="center">
<img src="build/icon.png" alt="Bonzai" width="128" height="128">
<h1>Bonzai - AI Orchestration Platform</h1>
</div>

![GitHub repo size](https://img.shields.io/github/repo-size/bon-zai/Dive)
![GitHub last commit](https://img.shields.io/github/last-commit/bon-zai/Dive?color=red)

**Bonzai** is a next-generation AI orchestration platform with **Zai** as your primary AI agent. Built on the Model Context Protocol (MCP), Bonzai provides a clean, powerful chat experience with voice integration, multi-agent orchestration, and 87+ tools.

## Features 🎯

### Current (Phase 1)
- 💬 **Clean Chat Interface**: Simple, beautiful conversation with Zai
- 🧠 **Claude Sonnet 4.5**: Powered by Anthropic's latest model
- 🔧 **MCP Integration**: 4 core tools ready (Mem0, Desktop Commander, Brave Search, Chrome DevTools)
- 💻 **Cross-Platform**: Windows, macOS, Linux (Tauri + Electron)
- 🌍 **Multi-Language**: 24+ languages supported

### Coming Soon
- 🎤 **Voice Integration**: Hume EVI for empathic voice interactions
- 🤝 **Multi-Agent Orchestration**: CrewAI + Minimax M2 integration
- 📚 **Persistent Memory**: Mem0 for conversation continuity
- 🖥️ **Desktop Commander**: Full desktop control via voice/chat
- 🌐 **87+ MCP Tools**: Comprehensive tool ecosystem

## What is Zai?

**Zai** is your AI agent personality - professional but warm, direct and action-oriented, sarcastic but loyal. Think of it as your AI companion that remembers conversations, controls your desktop, and gets things done without being verbose.

## Architecture

- **Frontend**: React + TypeScript
- **Desktop Framework**: Tauri (primary), Electron (fallback)
- **Backend**: Rust (Tauri) + Python (MCP Host)
- **State Management**: Jotai (atoms-based)
- **AI Integration**: Claude API (Anthropic)
- **Protocol**: Model Context Protocol (MCP)

## Development Setup

### Prerequisites
- Node.js 18+
- Rust (for Tauri builds)
- Python 3.12+ (for MCP host)

### Quick Start

```bash
# Clone the repository
git clone --recursive https://github.com/bon-zai/Dive.git
cd Dive

# Install dependencies
npm install

# Run in development mode (Tauri)
npm run dev:tauri

# Run in development mode (Electron)
npm run dev
```

### Build

```bash
# Build for production (Tauri)
npm run build

# Build for production (Electron)
npm run build:electron
```

## Configuration

### API Keys
Set up your Claude API key in Settings:
1. Launch Bonzai
2. Open Settings (⚙️)
3. Add your Anthropic API key

### MCP Tools
Configure MCP servers in `.config/mcp_config.json`:

```json
{
  "mem0": {
    "command": "cmd",
    "args": ["/c", "npx", "-y", "@mem0/mcp-server"],
    "env": {
      "MEM0_API_KEY": "your_mem0_key",
      "DEFAULT_USER_ID": "your_user_id"
    }
  }
}
```

See [MCP_SETUP.md](MCP_SETUP.md) for detailed configuration.

## Project Roadmap

### ✅ Phase 1: Foundation (Current)
- Complete rebrand from Dive to Bonzai/Zai
- Single-agent chat experience
- 4 core MCP tools operational
- Clean, simplified UI

### 🔄 Phase 2: Voice Integration
- Hume EVI integration
- Voice input/output
- Real-time audio streaming
- Voice state management

### 🔜 Phase 3: Multi-Agent Orchestration
- CrewAI integration
- Minimax M2 for advanced agents
- Background agent management
- Agent coordination layer

### 🔜 Phase 4: Advanced Features
- 87+ MCP tools ecosystem
- Advanced memory management (Mem0)
- Desktop Commander full integration
- Custom agent personalities

## Credits

Bonzai is built on the foundation of [Dive](https://github.com/OpenAgentPlatform/Dive), an open-source MCP Host Desktop Application. We're grateful to the Open Agent Platform team for their excellent work on the MCP integration architecture.

## License

MIT License - see [LICENSE](LICENSE) for details

## Contributing

Contributions welcome! Please read our contributing guidelines before submitting PRs.

## Support

- 📧 Issues: [GitHub Issues](https://github.com/bon-zai/Dive/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/bon-zai/Dive/discussions)

---

**Built with ❤️ by the Bonzai team**
