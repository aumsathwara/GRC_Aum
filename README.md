# Model Context Protocol (MCP)

## Overview

This repository provides a comprehensive implementation of the Model Context Protocol (MCP), a standardized interface for connecting AI applications with external tools and data sources. Designed to solve the MxN integration problem in AI systems, it offers:

- **Architecture Deep Dives**: Technical specifications and protocol documentation ([MCP - Architecture Deep Dive.md])
- **Production-Grade Implementations**: Ready-to-use MCP servers/clients for filesystems, weather APIs, and more
- **SDK \& Development Tools**: Python SDK with CLI support and Claude Desktop integrations
- **Enterprise Deployment**: Docker/IOWarp configurations for scalable deployments

## Features

**Core Protocol Features**

- JSON-RPC 2.0 communication with stateful connections
- Client-server architecture with capability negotiation
- Three server primitives: Tools, Resources, and Prompts
- Built-in security constraints and path validation

**Implementation Highlights**


| Component | Capabilities | Example Use Cases |
| :-- | :-- | :-- |
| FileSystem MCP | Secure read/write/search operations with path validation | Document management, Code analysis |
| Weather MCP | Real-time forecast/alerts via NWS API | Travel assistants, Event planning |
| Python SDK | Rapid server development with FastMCP class | Custom tool creation, API integrations |

## Prerequisites

- Python 3.9+
- [UV package manager](https://github.com/astral-sh/uv)
- Docker (for IOWarp deployments)
- API Keys: [Gemini](https://ai.google.dev/) / [Claude](https://docs.anthropic.com/claude/docs)


## Installation

```bash
git clone https://github.com/aumsathwara/GRC_Aum.git
cd GRC_Aum
```

**Key Implementations Setup**

1. **FileSystem MCP**
```bash
cd MCP Implementation/FileSystem MCP
uv add "mcp[cli]"
uv run server.py
```

2. **Python SDK**
```bash
cd MCP%20Python%20SDK
uv pip install -r requirements.txt
```

3. **IOWarp Deployment**
```bash
cd MCP on IOWarp
docker compose up -d
```


## Usage

**Basic Workflow**

1. Start MCP Server
```bash
uv run mcp dev MCP Implementation/FileSystem MCP/Server/server.py
```

2. Connect via Client
```python
from mcp.client import ClientSession
session = ClientSession.connect("filesystem://localhost:8000")
```

3. Use MCP Inspector
```
http://localhost:8000/status
```

**Claude Desktop Integration**

```json
{
  "mcpServers": {
    "FileSystem": {
      "command": "uv",
      "args": ["run", "server.py"]
    }
  }
}
```


## Future Roadmap

- Multi-protocol support (gRPC/WebSockets)
- Enhanced RBAC security model
- Vector database integrations
- Automated compliance auditing
- Distributed tracing system


## Contributing

We welcome contributions through:

1. **New MCP Implementations**: Create servers for novel use cases
2. **Protocol Enhancements**: Improve error handling/security
3. **Documentation**: Expand tutorials and API references

**Process**

1. Fork repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes
4. Push to branch
5. Open PR with detailed documentation

For major changes, please open an issue first to discuss proposed changes.
