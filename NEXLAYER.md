# Nexlayer — filebrowser

<!-- nexlayer:meta version=1 analyzed=2026-06-26T20:32:14Z repo=https://github.com/armondhonore/filebrowser branch=master -->

> **For AI agents (Claude Code, Cursor, Gemini CLI, Copilot):**
> This file is the **project context** for this Nexlayer deployment — tech stack, env vars, secrets, live URL.
> For full platform detail (nexlayer.yaml schema, Dockerfile rules, CI/CD, task recipes) read **`nexlayer.skills`** in this repo.
>
> **Critical rules (full detail in `nexlayer.skills`):**
> - Inter-pod refs: `${podName:port}` only — never `localhost` or bare hostnames
> - Docker Hub images: prefix with `mirror.gcr.io/library/` — bare tags fail on the cluster
> - Secrets: set in the Nexlayer dashboard — never commit to `nexlayer.yaml` or Dockerfile
>
> **This file:** `agent-managed` sections update automatically. `user-editable` sections (Local Development Setup, Nexlayer Deployment Plan, Build Notes) are yours — preserved across re-analysis.

## Project Summary
<!-- nexlayer:section agent-managed=project_summary -->
File Browser is a self-hosted, single-binary web interface for managing files within a specified directory, allowing users to upload, delete, preview, and edit files via a web UI.
<!-- nexlayer:end -->

## Technology Stack
<!-- nexlayer:section agent-managed=tech_stack -->
| Name | Kind | Version | Detected From |
|------|------|---------|---------------|
| Go | language | 1.25.0 | go.mod |
| bbolt | database | 1.4.3 | go.mod |
| BusyBox | infra | 1.37.0-musl | Dockerfile |
| tini | tool | static | Dockerfile |
<!-- nexlayer:end -->

## Repository Structure
<!-- nexlayer:section agent-managed=structure_map -->
- cmd/ — Entry points for the application
- frontend/ — Web interface assets and source
- storage/ — File system abstraction and storage logic
- users/ — User management and authentication
- auth/ — Authentication handlers and middleware
- http/ — HTTP server and routing implementation
<!-- nexlayer:end -->

## External Services Required
<!-- nexlayer:section agent-managed=external_deps -->
_No external services detected._
<!-- nexlayer:end -->

## Local Development Setup
<!-- nexlayer:section user-editable=local_setup -->
### Prerequisites

- Go >= 1.25.0
- Make or Taskfile

### Environment variables

Copy `.env.example` to `.env.local` and fill in:

```
FB_DATABASE=/path/to/database.db
FB_ROOT=/path/to/files
```

### Steps

1. `go mod download` — Fetch Go dependencies
2. `go build -o filebrowser main.go` — Build the single binary
3. `./filebrowser` — Start the server on default port 8080

<!-- nexlayer:end -->

## Nexlayer Setup
<!-- nexlayer:section agent-managed=nexlayer_setup -->
### Pod Environment Variables

| Pod | Variable | Value | Kind |
|-----|----------|-------|------|
| `app` | `PORT` | `"80"` | plain |

### nexlayer.yaml

```yaml
application:
  name: filebrowser
  pods:
    - name: app
      image: "registry.nexlayer.io/user_01kece1xyh817dwff7wnarhkxd/filebrowser:9f05a1b-fix4"
      path: /srv
      servicePorts:
        - 80
      vars:
        PORT: "80"
```

<!-- nexlayer:end -->

## Nexlayer Deployment Plan
<!-- nexlayer:section user-editable=deployment_plan -->
### Pod Topology

| Pod | Image | Port | Role |
|-----|-------|------|------|
| filebrowser | mirror.gcr.io/library/busybox:1.37.0-musl | 80 | web |
| filebrowser-db | mirror.gcr.io/library/busybox:1.37.0-musl | 0 | database |

### Deployment notes

- The application uses bbolt (embedded KV store), but according to Nexlayer rules, the database volume is isolated. The app connects to the database pod via filebrowser-db.pod
- The image uses a non-root user (UID 1000) for security
- Persistence is required for /srv (files), /config (settings), and /database (user data)

<!-- nexlayer:end -->

## Build Notes
<!-- nexlayer:section user-editable=build_notes -->
<!-- Add notes for future builds here — preserved across re-analysis -->
<!-- nexlayer:end -->

## Nexlayer Configuration
<!-- nexlayer:section agent-managed=nexlayer_config -->
**Last deployed:** 2026-06-26T21:06:36Z  
**Live URL:** https://relaxed-weasel-filebrowser.cloud.nexlayer.ai  
**Runtime:** go · **Port:** 80  
**Deploy branch:** master  

```yaml
application:
  name: filebrowser
  pods:
    - name: app
      image: "registry.nexlayer.io/user_01kece1xyh817dwff7wnarhkxd/filebrowser:9f05a1b-fix4"
      path: /srv
      servicePorts:
        - 80
      vars:
        PORT: "80"
```
<!-- nexlayer:end -->

## Build History
<!-- nexlayer:section agent-managed=build_history -->
| Date | Status | Notes |
|------|--------|-------|
| 2026-06-26T20:32:14Z | analyzed | initial repo analysis |
| 2026-06-26T21:06:36Z | success | deployed https://relaxed-weasel-filebrowser.cloud.nexlayer.ai |
<!-- nexlayer:end -->
