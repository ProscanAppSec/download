# Proscan Launcher

On-premises application security platform. The launcher is a native desktop application that manages the entire Proscan deployment on your machine.

## Download

**[Download Proscan Launcher (Windows x64)](https://github.com/ProscanAppSec/download/releases/latest/download/proscan.exe)** — ~16 MB

## How It Works

### Architecture

The launcher is built with [Wails](https://wails.io/) (Go + React). It provides a native desktop window that handles authentication, setup, package management, and hosts the full Proscan web interface inside the application.

```
┌──────────────────────────────────────────────────┐
│  Proscan Launcher (Wails — Go + React)           │
│                                                  │
│  ┌──────────────┐   ┌─────────────────────────┐  │
│  │ Launcher UI  │   │  Embedded Backend UI    │  │
│  │ (React SPA)  │   │  (proxied from Docker)  │  │
│  └──────┬───────┘   └────────┬────────────────┘  │
│         │                    │                    │
│  ┌──────▼────────────────────▼──────────────┐    │
│  │  Go Backend (app.go)                      │    │
│  │  • Auth & License (PaidActive)            │    │
│  │  • Docker Compose orchestration           │    │
│  │  • Package download, decrypt, load        │    │
│  │  • Vault encryption (AES-GCM)             │    │
│  │  • Guardian anti-tamper monitoring         │    │
│  │  • Heartbeat & session management         │    │
│  └──────────────┬───────────────────────────┘    │
│                 │                                 │
│  ┌──────────────▼───────────────────────────┐    │
│  │  Docker Compose (3 containers)            │    │
│  │                                           │    │
│  │  ┌─────────────────────────────────────┐  │    │
│  │  │  gps-backend (goproscan-backend)    │  │    │
│  │  │  Port 18080 → 8001                  │  │    │
│  │  │  12 scanner modules, API, web UI    │  │    │
│  │  │  Memory: 8 GB / CPUs: 4.0           │  │    │
│  │  └─────────────────────────────────────┘  │    │
│  │  ┌──────────────┐  ┌──────────────────┐  │    │
│  │  │  gps-pg       │  │  gps-redis       │  │    │
│  │  │  PostgreSQL 17│  │  Redis 7         │  │    │
│  │  │  Port 15432   │  │  Port 16379      │  │    │
│  │  │  2 GB / 2 CPU │  │  768 MB / 0.5 CPU│  │    │
│  │  └──────────────┘  └──────────────────┘  │    │
│  └───────────────────────────────────────────┘    │
└──────────────────────────────────────────────────┘
```

### Step-by-Step Flow

1. **Launch** — Run `proscan.exe`. The launcher opens a native window.

2. **Authentication** — Sign in with your PaidActive account. The launcher validates your license against `paidactive.com`. If you have a valid session from a previous run, it auto-restores without re-login. Offline grace periods are supported for air-gapped environments.

3. **Setup Wizard** (first run only) — Configure database password, admin credentials, network access settings, and ports. A self-signed TLS certificate is generated if network access is enabled.

4. **Package Download** — The launcher requests a download token from the license server, downloads the encrypted `.gpkg` package (~500 MB), and decrypts it. The package contains Docker images for the backend, PostgreSQL, and Redis.

5. **Package Verification** — Every `.gpkg` file is cryptographically verified:
   - **Magic bytes** header validation
   - **RSA-PSS signature** (SHA-512) verified against an embedded public key
   - **AES-GCM decryption** with a per-download outer key
   - **SHA-512 content hash** integrity check
   - **Per-component keys** derived for each component inside the package

6. **Docker Image Loading** — Decrypted Docker images are loaded into the local Docker engine via `docker load`. No images are pulled from public registries — everything comes from the signed package.

7. **Service Orchestration** — The launcher generates a `docker-compose.yml` from an embedded template and starts three containers:
   - **gps-backend** — The Proscan backend (API + web UI + all 12 scanner modules)
   - **gps-pg** — PostgreSQL 17 Alpine (scan results, findings, configurations)
   - **gps-redis** — Redis 7 Alpine (job queue, caching)

8. **Backend Proxy** — The launcher proxies requests to the backend container. API calls go to `/api/`, the web interface is served under `/app/`. WebSocket connections are redirected directly to the backend port for real-time scan updates.

9. **Ongoing** — A heartbeat runs every 5 minutes to validate the license, refresh session tokens, and check for package updates. If the server detects a session conflict (another device), the launcher logs out automatically.

### Security Model

- **No source code exposure** — The backend ships as pre-built Docker images inside an encrypted, signed package. No source code is distributed.
- **Package signing** — RSA-4096 PSS signatures with SHA-512. The public key is embedded in the launcher binary.
- **Vault encryption** — Components are encrypted at rest using AES-256-GCM with server-bound keys. Keys are derived per-component.
- **Anti-tamper** — Guardian module monitors for debuggers and runtime tampering. The launcher exits immediately if interference is detected.
- **Container hardening** — All containers run with `no-new-privileges`, dropped capabilities, read-only root filesystems, memory/CPU limits, and PID limits.
- **Config encryption** — Local settings are encrypted with a device-fingerprint-derived key using AES-GCM.
- **Single instance** — Platform-specific mutex (Windows) / flock (macOS/Linux) prevents multiple launcher instances.

### Requirements

- **Docker Desktop** (Windows/macOS) or **Docker Engine** (Linux). Podman is also supported.
- **8 GB RAM** minimum (16 GB recommended — backend alone uses up to 8 GB)
- **10 GB disk** for Docker images and scan data
- **Windows 10+**, **macOS 12+**, or **Linux** (x64)

### Closing Behavior

When closing the launcher, you choose:
- **Keep Running** — Closes only the launcher window. Docker containers continue running. Scans in progress complete normally.
- **Shutdown** — Stops all Docker containers and closes the launcher.

### Updates

The launcher checks for package updates via heartbeat. When an update is available:
1. Services are stopped (scan data and configurations are preserved in Docker volumes)
2. New package is downloaded and verified
3. Old Docker images are replaced with new ones
4. Services restart with the updated images

All user data (PostgreSQL database, scan results, findings, configurations) survives updates — only the Docker images are replaced.

### Backup & Restore

Built-in backup/restore via the launcher UI. Backups are password-encrypted `.gpbak` files containing the PostgreSQL database dump.

---

## Documentation

- [Technical Documentation](https://github.com/ProscanAppSec/docs)
- [Scan Results](https://github.com/ProscanAppSec/scan-results)
- [OWASP Benchmark Scorecard](https://github.com/ProscanAppSec/docs/blob/main/benchmark/results/OWASP_BENCHMARK_SCORECARD.md)

## Contact

- Email: [contact@proscan.one](mailto:contact@proscan.one)
- Website: [proscan.one](https://proscan.one)
