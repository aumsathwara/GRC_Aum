# MCP Server Docker for IOWarp 

This document describes how to expose the `/test` directory inside existing IOWarp container to an MCP-based docker service over Server-Sent Events (SSE). With this setup:

- **IOWarp** continues unmodified, writing its data into a Docker volume.
- A **custom‐mcp‐server** container mounts that same volume, runs your `server.py` over SSE, and exposes file-tools (`read_file`, `write_file`, `search_file`) at `http://localhost:8001/sse`.
- Your **client.py** then speaks SSE to `http://localhost:8001/sse` to perform file operations.

---

## Prerequisites

- Docker & Docker Compose installed on your host.
- A running IOWarp container (via its own Compose or `docker run`) with a non-empty `/test` directory.
- Your MCP codebase containing:
  - `server.py` (FastMCP file-service implementation)
  - `requirements.txt` (listing `mcp`, etc.)
  - A `Dockerfile` for building the sidecar image
  - `client.py` (updated to use SSE against `/sse`)

---

## 1. Share `/test` via a Named Volume

1. **Edit your IOWarp Compose**  
   In your IOWarp project’s `docker-compose.yml`, add:

   ```yaml
   services:
     iowarp:
       image: iowarp/iowarp-user:latest
       container_name: iowarp
       network_mode: host
       # … other settings …
       volumes:
         - iowarp_data:/test    # ← mount a named volume at /test

   volumes:
     iowarp_data:             # ← declare the volume
    ```

2. **Restart IOWarp**

    ```bash
    cd /path/to/iowarp-docker
    docker-compose down
    docker-compose up -d
    ```

This creates (or reuses) a Docker volume named iowarp_data, containing whatever lives in /test inside the IOWarp container.

---

## 2. Configure the MCP Sidecar Service

1. **Reference the existing volume externally**

    In your project’s docker-compose.yml:

    ```
    services:
    mcp-server:
        build: .
        container_name: custom-mcp-server
        ports:
        - "8001:8001"
        volumes:
        - iowarp_data:/mnt/iowarp/test    # ← mount the same volume
    volumes:
    iowarp_data:
        external: true                     # ← reuse the existing volume
    ```

    Note: If your IOWarp Compose gave the volume a different name (e.g. iowarp-docker_iowarp_data), add:

    ```
    volumes:
    iowarp_data:
        external: true
        name: iowarp-docker_iowarp_data
    ```

2. **Build & launch the project**

    ```bash
    cd /path/to/custom-mcp
    docker-compose down
    docker-compose build
    docker-compose up -d
    ```
    The sidecar image will start FastMCP bound to 0.0.0.0:8001, serving SSE events at /sse.

---

## 3. Verify the Setup

1. **Check SSE endpoint**
    ```bash
    curl -I http://localhost:8001/sse
    # Expect: HTTP/1.1 200 OK
    ```

2. **Inspect mounted data**

    ```bash
    docker exec -it custom-mcp-server bash
    ls -l /mnt/iowarp/test
    # Should list the same files you saw in IOWarp’s /test
    ```

3. **Run your client**

    ```bash
    python client.py
    ```
    You should see:
    ```bash
    Connected via SSE; tools: ['read_file','write_file','search_file']
    MCP Client Started! (type 'quit' to exit)
    ```

---

## 4. How It Works

1. **IOWarp Container**

    - Writes or updates files under /test
    - Those files are persisted in the Docker volume iowarp_data.

2. **MCP Sidecar Container**

    - Mounts iowarp_data at /mnt/iowarp/test

    - Runs server.py, which:

        - Initializes a FastMCP(name="FileSystem", host="0.0.0.0", port=8001)

        - Registers three tools:

           - read_file(path) → returns file text/base64

            - write_file(path, content) → appends to files

           -  search_file(ext, search_dir) → glob search

    - Starts an SSE server (mcp.run(transport="sse")) on /sse.

3. **Local Client (client.py)**

    - Connects over SSE to http://localhost:8001/sse

    - Discovers the three tools, then invokes them via SSE messages

    - Parses and prints out file contents or listings.

--- 

## 5. Troubleshooting

- 404 on /sse

    - Ensure server.py omits invalid path= or port= args to run()

    - Confirm FastMCP constructor includes host="0.0.0.0", port=8001.

- Empty directory in sidecar

    - Check you declared the volume external: true and used the correct name:

    - Run docker volume ls to find the real volume name.

- SSE connection refused

    - Remove network_mode: host and rely on ports: mapping

    - Rebuild & restart sidecar so it binds on container’s internal interface.
