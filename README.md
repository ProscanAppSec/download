# Proscan

On-premises application security platform. Your code never leaves your machine.

## Install

One command. Any platform with Docker.

```bash
docker run -d \
  --name proscan \
  --restart unless-stopped \
  -p 9090:9090 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  registry.proscan.one:2083/proscan/launcher
```

**Windows PowerShell:**
```powershell
docker run -d --name proscan --restart unless-stopped -p 9090:9090 -v //var/run/docker.sock:/var/run/docker.sock registry.proscan.one:2083/proscan/launcher
```

Then open **http://localhost:9090** in your browser.

## Prerequisites

1. **Docker Desktop** (Windows/macOS) or **Docker Engine** (Linux) — installed and running

2. **Add the Proscan registry** to Docker's insecure registries (one-time):

   **Docker Desktop (Windows / macOS):**
   - Open Docker Desktop → Settings → Docker Engine
   - Add to the JSON config:
   No special Docker configuration needed — the registry runs on port 2083 with TLS.

3. **System resources:** 8 GB RAM minimum (16 GB recommended), 4 CPU cores, 20 GB disk

## Getting Started

1. **Open the launcher** — Go to **http://localhost:9090** in your browser
2. **Create account** — Register with your email and password
3. **Activate license** — Start a 15-day free trial or purchase a plan
4. **Setup wizard** — Configure database password, admin credentials, network settings, and ports
5. **Start scanning** — The launcher pulls the backend, starts all services, and you're ready to go

Once services are running, open **http://localhost:18080** or click "Open ProScan" in the dashboard.

## What Gets Deployed

The launcher orchestrates three containers on your machine:

| Container | Purpose |
|-----------|---------|
| **Backend** | Proscan API, web interface, and all 12 scanner modules |
| **PostgreSQL 17** | Scan results, findings, and configurations |
| **Redis 7** | Job queue and caching |

Everything runs locally. No code or data leaves your network.

## Managing

Access the launcher dashboard at **http://localhost:9090** to:
- View service status
- Start / Stop / Restart services
- Check for updates
- Create and restore backups
- Configure container resources
- Report issues

### Stop

```bash
# Stop launcher only (backend keeps running)
docker stop proscan

# Stop everything
docker stop proscan gps-backend gps-pg gps-redis
```

### Start

```bash
docker start proscan
```
Then open http://localhost:9090 and click "Start Services".

### Update

From the dashboard, click **"Check Updates"**. If available, click **"Update"**. The launcher pulls the new version and restarts services. All scan data and configurations are preserved.

### Uninstall

```bash
docker stop proscan gps-backend gps-pg gps-redis
docker rm proscan gps-backend gps-pg gps-redis
docker volume rm proscan-data goproscan_goproscan_pgdata goproscan_goproscan_redis goproscan_goproscan_data
docker rmi registry.proscan.one:2083/proscan/launcher:latest registry.proscan.one:2083/proscan/backend:latest
```

## Ports

| Port | Service | Default |
|------|---------|---------|
| 9090 | Launcher dashboard | localhost only |
| 18080 | Proscan web interface | localhost only |

## Troubleshooting

**"Docker is not installed"** — Install Docker Desktop from https://docs.docker.com/desktop/

**Services won't start** — Ensure Docker has at least 8 GB RAM and 4 CPUs allocated (Docker Desktop → Settings → Resources).

**Backend takes a long time on first start** — Database migrations run on first launch (60-90 seconds). Wait for the healthcheck to pass.

**Port already in use** — Start with a different port:
```bash
docker run -d --name proscan -p 9091:9090 -v /var/run/docker.sock:/var/run/docker.sock registry.proscan.one:2083/proscan/launcher
```

---

## Documentation

- [Technical Documentation](https://github.com/ProscanAppSec/docs)
- [Scan Results](https://github.com/ProscanAppSec/scan-results)
- [OWASP Benchmark Scorecard](https://github.com/ProscanAppSec/docs/blob/main/benchmark/results/OWASP_BENCHMARK_SCORECARD.md)

## Support

- **In-App:** Use the "Report Issue" button in the launcher dashboard
- **Email:** [support@proscan.one](mailto:support@proscan.one)
- **Website:** [proscan.one](https://proscan.one)
