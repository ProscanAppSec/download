# Proscan Launcher

On-premises application security platform. The launcher is a native desktop application that manages the entire Proscan deployment on your machine.

## Download

| Platform | Download | Size |
|----------|----------|------|
| **Windows x64** | [ProScan-windows-amd64.exe](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-windows-amd64.exe) | 11.1 MB |
| **Linux x64** | [ProScan-linux-amd64](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-linux-amd64) | 10.5 MB |
| **macOS Intel** | [ProScan-darwin-amd64](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-darwin-amd64) | 10.6 MB |
| **macOS Apple Silicon** | [ProScan-darwin-arm64](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-darwin-arm64) | 10.4 MB |

On Linux/macOS, make it executable after downloading: `chmod +x ProScan-*`

## How It Works

### Step-by-Step

1. **Launch** — Run `proscan.exe`. A native desktop window opens.

2. **Sign In** — Authenticate with your PaidActive account. If you've logged in before, the session restores automatically. Offline use is supported within your license period.

3. **Setup** (first run only) — A setup wizard walks you through configuring admin credentials, ports, and network settings.

4. **Package Install** — The launcher downloads the Proscan backend package, verifies its integrity and authenticity, and installs it locally. Everything runs on your machine — no external image pulls.

5. **Start Scanning** — Three local services start automatically: the Proscan backend (API + web UI + all 12 scanner modules), a PostgreSQL database, and a Redis cache. The full web interface loads inside the launcher window.

6. **Ongoing** — The launcher periodically validates your license and checks for updates. Updates are applied in-place — your scan results, findings, and configurations are preserved.

### Architecture

The launcher is a native desktop application built with [Wails](https://wails.io/) (Go + React). It orchestrates three Docker containers locally:

- **Backend** — Proscan API, web interface, and all 12 scanner modules
- **PostgreSQL 17** — Stores scan results, findings, and configurations
- **Redis 7** — Job queue and caching

The launcher window embeds the full Proscan web interface, so there's no need to open a browser. WebSocket connections are supported for real-time scan progress.

### Requirements

- **Docker Desktop** (Windows/macOS) or **Docker Engine** (Linux). Podman is also supported.
- **8 GB RAM** minimum (16 GB recommended)
- **10 GB disk** for installation and scan data
- **Windows 10+** (x64), **macOS 12+** (Intel or Apple Silicon), or **Linux** (x64)

### Closing Behavior

When closing the launcher, you choose:
- **Keep Running** — Closes only the launcher window. Containers keep running. Scans in progress complete normally.
- **Shutdown** — Stops all containers and closes the launcher.

### Updates

When a new version is available, the launcher downloads and installs it automatically. All user data (scan results, findings, configurations) is preserved across updates — only the backend images are replaced.

### Backup & Restore

Built-in backup and restore through the launcher UI. Backups are password-encrypted files containing your full database.

---

## Documentation

- [Technical Documentation](https://github.com/ProscanAppSec/docs)
- [Scan Results](https://github.com/ProscanAppSec/scan-results)
- [OWASP Benchmark Scorecard](https://github.com/ProscanAppSec/docs/blob/main/benchmark/results/OWASP_BENCHMARK_SCORECARD.md)

## Contact

- Email: [contact@proscan.one](mailto:contact@proscan.one)
- Website: [proscan.one](https://proscan.one)
