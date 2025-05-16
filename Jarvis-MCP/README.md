
# Jarvis-MCP

*A python package with Gemini client + MCP server to interact with [Jarvis](https://grc.iit.edu/docs/jarvis/jarvis-cd/index/)*

---

## Overview

**Jarvis-MCP** is a Python package that allows you to control Jarvis using a Gemini-powered client and a Model Context Protocol (MCP) server.

With **Jarvis-MCP**, you can:

* Initialize and configure Jarvis
* Bootstrap from ARES, local or other machine 
* Create, list, run, and destroy pipelines
* Add or remove packages from a pipeline
* Update package configurations
* Build and run pipelines

---

## Directory Structure

```
Jarvis-MCP/
├── README.md              # ← This file
├── GUIDE.md               # Details how to setup and install
├── pyproject.toml         # Python package config
├── requirements.txt       # All dependencies (incl. GitHub links)
├── uv.lock                # Dependency lock file
└── src/
    └── jarvis_mcp/
        ├── client.py      # Gemini-based interactive client
        ├── server.py      # MCP server for Jarvis tools
        └── capabilities/
            └── jarvis_handler.py  # Actual functions that control Jarvis
```

---

## How to Use

To run **Jarvis-MCP** on **ARES** or any **Node**, use:

```bash
python3 [path/to/client.py] --server-script [path/to/server.py]
```

Example:

```bash
python3 src/jarvis_mcp/client.py --server-script src/jarvis_mcp/server.py
```

This command will start the client and connect it to the MCP server.

"OR" simply type
```bash
mcp-server 
```
in one terminal and 

```bash
mcp-client 
```
in another. 

Refer to Installation and setup [guide](./GUIDE.md) for in detail explanation.  

---

## Operations and Screenshots

### 1. **Initialize Jarvis**

The first step is to initialize Jarvis. This prepares the system for interaction.

```bash
# Command to initialize Jarvis
Query: Initialize jarvis with configur, private and shared dir as " . /jarvis—pipelines'
```

**Output Screenshot**

![Screenshot of initializing jarvis using Gemini client](<assets/Screenshot 2025-05-15 160800.png>)

---

### 2. **Create Pipeline (`ior_test`) and append package**

Create a new pipeline named `ior_test` and append package to it. This will be used for testing purposes.

```bash
# Command to create a pipeline
Query: create a pipeline called ior_test and append package ior to it
```

**Output Screenshot**

![alt text](<assets/Screenshot 2025-05-15 162219.png>)

---

### 3. **Change Configuration of Added Package**

You can also see and modify the configuration of the package you've added to the pipeline.

```bash
# Command to change the configuration
Query: show the configuration of ior in ior_test
```

**Output Screenshot**

![alt text](<assets/Screenshot 2025-05-15 162322.png>)

```bash
# Command to change the configuration
Query: update the nprocs to 8 for package ior in pipeline ior_test
```

![alt text](<assets/Screenshot 2025-05-15 162545.png>)
---

### 4. **Build Environment for `ior_test` Pipeline**

After configuring the pipeline, you can build the environment for `ior_test`.

```bash
# Command to build the environment
Query: Build environment for pipeline ior_test 
```

**Output Screenshot**

![alt text](<assets/Screenshot 2025-05-15 162922.png>)

---

### 5. **Run the Pipeline (`ior_test`)**

Finally, you can run the pipeline to see everything in action.

```bash
# Command to run the pipeline
Query: select the pipeline ior_test and run it
```

**Output Screenshot**

![alt text](<assets/Screenshot 2025-05-15 163023.png>)

---

or **Just write below**:
```bash
Query: create a pipeline called ior_test_2. Add package ior with nprocs set to 16. After adding, set the pipeline ior_test_2 as current and build environment for it and run it.
```

**Output Screenshot**

![alt text](<assets/Screenshot 2025-05-15 163759.png>)

---
## Notes

* Ensure your environment is set up with Python 3.10+
* You’ll need an `.env` file if you're using the Gemini API directly
* Use `pip install -e .` in the repo to enable CLI tools like `mcp-client` or `mcp-server` (optional)
