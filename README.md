# Proscan Launcher

On-premises application security platform. The launcher is a native desktop application that manages the entire Proscan deployment on your machine.

## Download

| Platform | Download | Size |
|----------|----------|------|
| **Windows x64** | [ProScan-windows-amd64.exe](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-windows-amd64.exe) | 16.3 MB |
| **Linux x64** | [ProScan-linux-amd64](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-linux-amd64) | 15.7 MB |
| **macOS Universal (DMG)** | [ProScan-1.0.0-macos-universal.dmg](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-1.0.0-macos-universal.dmg) | 23.7 MB |
| **macOS Universal (ZIP)** | [ProScan-1.0.0-macos-universal.zip](https://github.com/ProscanAppSec/download/releases/latest/download/ProScan-1.0.0-macos-universal.zip) | 15.6 MB |

The macOS DMG and ZIP both contain a universal binary that runs natively on Intel and Apple Silicon Macs.

## Quick Start

**Windows** — Download and run `ProScan-windows-amd64.exe`.

**Linux** — Download, make executable, and run:
```bash
chmod +x ProScan-linux-amd64
./ProScan-linux-amd64
```

**macOS (DMG)** — Open the DMG, drag Proscan to Applications. On first launch, macOS may show a Gatekeeper warning. Bypass with:
```bash
xattr -d com.apple.quarantine /Applications/ProScan.app
```

**macOS (ZIP)** — Extract and run. Same Gatekeeper bypass applies if needed.

## How It Works

1. **Launch** — Run the launcher. A native desktop window opens.

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

- **Docker Desktop** (Windows/macOS) or **Docker Engine** (Linux)
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
