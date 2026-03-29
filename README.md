# Download Proscan

Start your 7-day free trial. No credit card required. Full access to all 12 scanners.

## Download

**[Download Proscan for Windows (64-bit)](https://github.com/Proscan-hub/download/releases/latest/download/proscan-windows-amd64.exe)**

## What Happens After You Download

### Step 1: Launch

Run `proscan-windows-amd64.exe`. The Proscan application window opens — this is a native desktop application, not a browser.

### Step 2: Create an Account or Sign In

- **New users:** Click Register, enter your email, password, and name. Your 7-day free trial starts immediately upon registration.
- **Returning users:** Sign in with your existing credentials.

### Step 3: Package Download

After signing in, Proscan automatically downloads the scanner package. This is a one-time download (~200-500 MB depending on platform) that includes the full scanning engine, database, and all tools. Progress is shown in the application window.

### Step 4: Setup Wizard (First Run Only)

The setup wizard walks you through initial configuration:

1. **System check** — verifies your machine meets the minimum requirements
2. **Database password** — a secure password is generated automatically
3. **Admin account** — create your admin username, password, and email for the web interface
4. **Network access** — choose whether Proscan is accessible only locally or over your network
5. **Port configuration** — set ports for the web UI, database, and cache (defaults work for most setups)
6. **Review** — confirm your settings
7. **Install** — Proscan sets up all required services automatically

### Step 5: Services Start

Proscan starts all required services (database, cache, scanner backend) automatically. The status dashboard in the application window shows the health of each service.

### Step 6: Open the Scanner

Click "Open Proscan" in the application window. The full Proscan web interface loads inside the application, where you can sign in with the admin credentials you created in Step 4 and start scanning.

### What's Running

Everything runs locally on your machine:

- **Scanner backend** — processes scans and manages findings
- **PostgreSQL database** — stores scan results and configuration
- **Redis cache** — handles real-time updates and task queuing
- **Web interface** — the Proscan UI displayed inside the application window

No source code, scan results, or telemetry data is sent externally. The only external communication is license validation during sign-in.

## System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| OS | Windows 10 (64-bit) or Server 2019 | Windows 11 or Server 2022 |
| CPU | 4 cores | 8 cores |
| RAM | 16 GB | 32 GB |
| Disk | 20 GB free | 50 GB+ free |
| Network | Required for initial sign-in and package download | Not required after setup (air-gapped supported) |

## What's Included in the Trial

Full access to all scanners for 7 days:

- **SAST** — source code analysis across 60+ languages
- **DAST** — dynamic web application testing
- **SCA** — open-source dependency scanning
- **Secrets** — credential and API key detection
- **IaC** — infrastructure-as-code scanning (Terraform, K8s, Docker, CloudFormation)
- **Container** — image vulnerability scanning
- **API** — REST and GraphQL endpoint testing
- **AI/LLM** — AI model security testing
- **Binary** — compiled binary and bytecode analysis
- **Network** — host discovery and port scanning
- **Code Quality** — complexity, duplication, and technical debt
- **ProScan Deep** — multi-phase scanning with built-in pen test toolkit

## After the Trial

When the 7-day trial ends, the application prompts you to sign in to renew or purchase a license. Your scan results and configuration are preserved — nothing is lost.

## Links

- Website: [proscan.one](https://proscan.one)
- Documentation: [Proscan-hub/docs](https://github.com/Proscan-hub/docs)
- Contact: [contact@proscan.one](mailto:contact@proscan.one)
