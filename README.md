# ReadME 

This repo contains all work related to **Model Context Protocol (MCP)** — including documentation, and working projects. It’s organized for anyone to explore and test.

---

## 📁 Folders Overview

### [`Documentation/`](./Documentation)

All notes and guides about MCP.

* **[How MCP works](./Documentation/MCP%20Deep%20Dive/)** – Basics, how it works, and key terms.
* **[Step-by-step examples](./Documentation/Basic%20MCP%20Implementation/)** – Tutorials with real code (like file system or weather tools).
* **[Using the Python MCP SDK](./Documentation/Basic%20MCP%20Implementation/MCP%20Python%20SDK/)** – How to use Python to make MCP tools and connect them to AI.

---

### [`Jarvis-MCP/`](./Jarvis-MCP)

This is a working MCP tool that connects to **Jarvis-CD pipelines**.

* Uses Gemini + FastAPI.
* Runs local or remote.
* Supports both standard output and SSE.

📄 [Jarvis-MCP README](./Jarvis-MCP/README.md)
📄 [How to install and setup](./Jarvis-MCP/GUIDE.md)

---

### [`Multiple MCP/`](./Multiple%20MCP)

A collection of MCP tools, each in its own folder:

* [`FileSystem-MCP`](./Multiple%20MCP/FileSystem-MCP) – Read and write files using MCP.
* [`Weather-MCP`](./Multiple%20MCP/Weather-MCP) – Get weather info using MCP.

You can install them one by one using their `pyproject.toml` "OR" all together.

---


## 📬 Contact

Maintained by Aum Vinodchandra Sathwara
GitHub: [@aumsathwara](https://github.com/aumsathwara)

