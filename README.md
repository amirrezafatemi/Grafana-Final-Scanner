# GRAFANA FINAL SCANNER v3.0

</br>

## Executive Summary

**Grafana Final Scanner** is a professional-grade security assessment tool designed for comprehensive vulnerability detection in Grafana deployments. It features multi-source version fingerprinting, version-aware CVE checking, configuration analysis, auto-detection of Grafana instances, vulnerability management, and a built-in web dashboard.

> [!WARNING]
> **This tool is intended for AUTHORIZED security assessments and educational purposes only.**
> 
> **By using this software, you agree to the following conditions:**
>    - **Only scan systems you own, manage, or have EXPLICIT WRITTEN PERMISSION from the owner to test.**
>    - **Never use this tool against any system without proper authorization. Unauthorized access, vulnerability scanning, or any form of security testing without explicit permission is ILLEGAL in most jurisdictions and may violate laws such as the Computer Fraud and Abuse Act (CFAA) and similar international statutes.**
>    - **This software is provided "AS IS" without any warranties, expressed or implied, regarding its accuracy, reliability, or suitability for any purpose. The developer assumes no liability for any misuse or damage caused by this tool.**
>    - **The user is solely responsible for obtaining all necessary authorizations and complying with all applicable laws and regulations before using this software.**

If you are unsure whether you have authorization to scan a target, **DO NOT USE THIS TOOL**. Stop and seek proper legal guidance.

</br>

## Table of Contents

- [Executive Summary](#executive-summary)
- [Key Features](#key-features)
- [Installation](#installation)
  - [Quick Start](#quick-start)
  - [For Web Dashboard](#for-web-dashboard)
- [Usage](#usage)
  - [Basic Commands](#basic-commands)
  - [Command-Line Options](#command-line-options)
  - [New Features in Detail](#new-features-in-detail)
- [Vulnerability Database](#vulnerability-database)
- [Sample Output](#sample-output)
- [Screenshots](#screenshots)
- [Technical Methodology](#technical-methodology)
- [Docker Usage](#docker-usage)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)

</br>

## Key Features

### Core Capabilities

- **15+ CVE Vulnerability Checks** - Comprehensive coverage from 2018-2025 with version-aware filtering
- **Auto-Search** - Automatically detect and scan Grafana instances from a file containing mixed URLs
- **Smart Version Detection** - Multi-endpoint fingerprinting with 7+ fallback strategies
- **Vulnerability Management** - Persistent JSON database for tracking targets, vulnerabilities, and scan history
- **Target Management** - Track scan count, vulnerability counts, risk scores per target
- **Built-in Web Dashboard** - View results, manage vulnerabilities, and track targets via Flask web UI
- **False Positive Reduction** - Strict content validation with multi-indicator verification
- **Parallel Scanning** - Configurable threading for high-speed batch assessments
- **Authentication Support** - Bearer token and Basic auth for internal targets
- **Multi-Format Reports** - JSON, HTML, and CSV output with severity visualization
- **Configuration Analysis** - Security headers, CORS, anonymous access, plugins, and more

</br>

### What's New in v3.0

- **Auto-Search (`--auto-search`)** - Detect Grafana instances from mixed URL lists with multi-method fingerprinting (API, HTML, headers)
- **Vulnerability Management (`--db`)** - Persistent JSON database with deduplication, status tracking, and risk scoring
- **Web Dashboard (`--serve`)** - Built-in Flask web server with dashboard, target management, and vulnerability management
- **15+ CVE Checks** - Extended coverage including CVE-2025-4123, CVE-2024-9264, CVE-2024-8118
- **Enhanced Grafana Detection** - 5 detection methods with confidence scoring
- **Risk Scoring** - Automatic calculation (0-100) based on vulnerability severity and count
- **Scan History** - Automatic tracking of all scan executions with duration and findings

</br>

## Installation

### Quick Start

```bash
pip install -r requirements.txt
git clone https://github.com/Zierax/Grafana-Final-Scanner.git
chmod +x scanner.py
python scanner.py -u https://grafana.example.com
```

</br>

### For Web Dashboard

```bash
pip install flask
python scanner.py --serve --db vulndb.json
```

</br>

## Usage

### Basic Commands

```bash
# Single target
python scanner.py -u https://grafana.example.com

# Batch scan with HTML report
python scanner.py -f targets.txt -o report

# Auto-detect Grafana from mixed URL list
python scanner.py --auto-search urls.txt -o discovery_report

# Verbose authenticated scan
python scanner.py -u https://grafana.target.com -v --auth-token "glsa_xxx"

# Vulnerability management with database
python scanner.py -f targets.txt --db vulndb.json

# Start web dashboard
python scanner.py --serve --db vulndb.json

# Web dashboard on custom port
python scanner.py --serve 9090 --host 0.0.0.0 --db vulndb.json

# Basic auth with parallel scanning
python scanner.py -u https://internal.grafana.local --auth-user admin --auth-pass password

# Self-signed SSL (internal targets)
python scanner.py -u https://grafana.internal.local --no-ssl-verify

# High-speed batch scan
python scanner.py -f targets.txt --threads 20 -o scan_results
```
</br>

### Command-Line Options

```
-u, --url              Single target URL
-f, --file             File with target URLs (one per line)
-o, --output           Save reports (JSON, HTML, CSV auto-generated)
-t, --timeout          HTTP timeout in seconds (default: 10)
--no-ssl-verify        Disable SSL certificate verification
-v, --verbose          Enable detailed logging (shows all checks)
--auth-token           Bearer token for authenticated scanning
--auth-user            Username for basic authentication
--auth-pass            Password for basic authentication
--threads              Max threads for parallel scanning (default: 5)
--db FILE              Enable vulnerability management with JSON database
--serve [PORT]         Start web dashboard (requires Flask, uses --db)
--auto-search FILE     Auto-detect Grafana instances from file containing mixed URLs
--host                 Host to bind web server to (default: 127.0.0.1)
--no-banner            Suppress banner display
```

</br>

### New Features in Detail

#### Auto-Search (`--auto-search`)

Quickly identify Grafana instances from a large file of mixed URLs:

```bash
python scanner.py --auto-search urls.txt -o discovery_report
```

The auto-search feature:
1. Reads all URLs from the specified file
2. Probes each URL using multiple detection methods (API health check, HTML analysis, endpoint probing, header analysis)
3. Assigns a confidence score to each potential Grafana instance
4. Scans only confirmed Grafana instances (confidence >= 30%)
5. Reports results with confidence scores and detected versions

#### Vulnerability Management (`--db`)

Persistent tracking of targets and vulnerabilities across scan sessions:

```bash
# First scan - creates database
python scanner.py -u https://grafana.example.com --db vulndb.json

# Second scan - adds to existing database
python scanner.py -u https://grafana2.example.com --db vulndb.json

# View database statistics
python scanner.py --serve --db vulndb.json
```

The database tracks:
- Target URLs with version history
- Vulnerability counts by severity
- Risk scores (0-100)
- Scan history with timestamps and durations
- Vulnerability status (open, fixed, false_positive, accepted)

#### Web Dashboard (`--serve`)

Built-in Flask web server for viewing and managing scan results:

```bash
# Start dashboard on default port 8080
python scanner.py --serve --db vulndb.json

# Start on custom port and host
python scanner.py --serve 9090 --host 0.0.0.0 --db vulndb.json
```

Features:
- Dashboard with statistics overview
- Target management with risk scores
- Vulnerability listing with severity badges
- One-click vulnerability status updates (fixed, false positive)
- Responsive design with dark theme

</br>

## Vulnerability Database

### Critical Severity

| CVE | CVSS | Description | Affected Versions |
|:---|:---:|:---:|:---:|
| CVE-2025-4123 | 8.2 | Path Traversal & Open Redirect XSS | < 12.0.0+security-01 |
| CVE-2024-9264 | 9.0+ | DuckDB SQL Injection (RCE) | 11.0.0-11.2.1 |
| CVE-2024-8118 | 9.0+ | OAuth Authentication Bypass | 11.0.x-11.2.1 |
| CVE-2021-43798 | 7.5 | Directory Traversal (File Read) | 8.0.0-8.3.0 |

</br>

### High Severity

| CVE | CVSS | Description | Affected Versions |
|:---|:---:|:---:|:---:|
| CVE-2023-50164 | 8.0 | Plugin Path Traversal | < 9.2.10, 9.3.x < 9.3.6 |
| CVE-2023-1410 | 8.8 | SSRF via Data Source Proxy | 8.0.0-9.2.17, 9.3.x < 9.3.5 |
| CVE-2023-2183 | 8.1 | Authentication Bypass | 8.x, 9.x before patches |
| CVE-2018-15727 | 8.1 | Auth Bypass (Cookie Forging) | 2.x-5.2.2 |
| CVE-2021-27358 | 7.5 | DoS via Snapshots API | 6.7.3-7.4.1 |

</br>

### Medium/Low Severity

| CVE | CVSS | Description | Affected Versions |
|:---|:---:|:---:|:---:|
| CVE-2024-1313 | 5.5 | Information Disclosure | Multiple versions |
| CVE-2021-39226 | 6.5 | Snapshot Enumeration | 8.0.0-8.3.0 |
| CVE-2020-11110 | - | Stored XSS | < 6.7.0 |
| CVE-2021-41174 | - | AngularJS XSS | 8.0.0-8.3.0 |
| CVE-2022-32275/32276 | - | v8.4.3 Specific Issues | 8.4.3 only |

</br>

### Configuration Checks

- **Anonymous Access** - Unauthenticated viewing enabled?
- **Metrics Exposure** - Prometheus endpoint publicly accessible?
- **Plugin Analysis** - Unsigned plugins detected?
- **Security Headers** - CSP, HSTS, XFO, XSS-Protection audit
- **CORS Configuration** - Wildcard/reflective CORS detected?
- **Self-Signup** - Unauthorized user registration enabled?
- **API Configuration** - Sensitive data in API responses?
- **Server Info Disclosure** - Build information leaked via health endpoint?

</br>

## Sample Output

```
    ╔═══════════════════════════════════════════════════════════════╗
    ║   ███████╗██████╗████████╗███████╗██╗██╗██╗     ╔══╗║
    ║   ██╔════╝██╔══████╔════╝██╔════╝██║██╔██╗██║     ║  ║║
    ║   █████╗  ██████╔███████╗█████╗  ██║████╗ ██║     ╠╝  ╚╣║
    ║   ██╔══╝  ██╔══██╔══██║██╔══╝  ██║██║╚██╗██║     ║   ╔╝║
    ║   ██║     ██║  ███████║███████╗██║██║ ╚████║     ║   ╚╗║
    ║   ╚═╝     ╚═╝  ╚══════╝╚══════╝╚═╝╚═╝  ╚═══╝     ╩═══╝║
    ║                                                              ║
    ║   ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓║
    ║   GRAFANA FINAL SCANNER    Professional Security Audit Suite     ║
    ║   v3.0.0 | 15 CVE Checks | Multi-Format Reports | Web Dashboard║
    ║   Developed by: Ziad                                         ║
    ╚═══════════════════════════════════════════════════════════════╝

╔══════════════════════════════════════════════════════════════════════╗
║ TARGET ASSESSMENT                                                    ║
║ https://grafana.example.com                                          ║
╚══════════════════════════════════════════════════════════════════════╝

ℹ [INFO] Phase 1: Connectivity Verification
  ✓ [OK] Target reachable (HTTP 200)

ℹ [INFO] Phase 2: Version Fingerprinting
  ✓ [OK] Version detected: Grafana v8.2.5

ℹ [INFO] Phase 3: Vulnerability Scanning

  🔴 [CRITICAL] CVE-2021-43798    Directory Traversal
     └─ Directory traversal CONFIRMED - File read via 'alertlist' plugin (3/8 indicators, 1245 bytes)
     └─ Test URL: https://grafana.example.com/public/plugins/alertlist/../../../../../../../../etc/passwd

  🟡 [MEDIUM] CVE-2024-1313    Information Disclosure
     └─ OAuth client ID exposed in frontend settings
     └─ Test URL: https://grafana.example.com/api/frontend/settings

ℹ [INFO] Phase 4: Security Configuration Analysis
  🟡 [MEDIUM] Anonymous access ENABLED - unauthenticated viewing possible
  ⚡ [WARN] CORS misconfiguration: Origin header reflected
  🔵 [LOW] Missing security headers (2): Content-Security-Policy, Strict-Transport-Security
```

</br>

## Screenshots

Below are three screenshots showing the tool in action:

![Alt text of the image](https://github.com/amirrezafatemi/Grafana-Final-Scanner/blob/main/assets/dashboard_page.jpg)
Figure 1: The main dashboard of the Grafana Final Scanner web interface

![Alt text of the image](https://github.com/amirrezafatemi/Grafana-Final-Scanner/blob/main/assets/target_page.jpg)
Figure 2: Detailed vulnerability list with their risk score per asset

![Alt text of the image](https://github.com/amirrezafatemi/Grafana-Final-Scanner/blob/main/assets/vulnerability_page.jpg)
Figure 3: The vulnerability management page, showing a filterable table of all discovered vulnerabilities

</br>

## Technical Methodology

### Scanning Process

1. **Connectivity Verification** - TCP/HTTP handshake and SSL validation
2. **Version Fingerprinting** - Multi-source detection from 7+ endpoints
3. **Vulnerability Assessment** - Version-aware CVE testing with strict validation
4. **Configuration Analysis** - Security posture evaluation (headers, CORS, auth)
5. **Results Compilation** - Aggregation, deduplication, and database persistence
6. **Report Generation** - Multi-format output (JSON, HTML, CSV)

### Grafana Detection (Auto-Search)

The auto-search feature uses a multi-stage detection pipeline:

1. **API Health Check** (highest confidence) - Check `/api/health` for Grafana-specific JSON
2. **HTML Analysis** - Look for Grafana indicators in login page content
3. **Frontend Settings** - Check `/api/frontend/settings` for buildInfo
4. **Endpoint Probing** - Test multiple Grafana-specific API endpoints
5. **Header Analysis** - Check response headers for Grafana signatures

A minimum confidence threshold of 30% is required to classify a URL as Grafana.

### False Positive Reduction

- **Version-Based Filtering**: Skip inapplicable CVE checks (~40% reduction)
- **Content Validation**: Require specific indicators, not just HTTP status (~60% reduction)
- **Multi-Vector Testing**: Test multiple variants for confirmation
- **Response Validation**: Content length, JSON structure, and indicator matching
- **Rate Limit Detection**: Prevents false negatives from rate-limited responses

</br>

### Vulnerability Management Database

The JSON database (`--db`) provides:

- **Target Tracking**: URL, version, first seen, last scanned, scan count
- **Vulnerability Records**: CVE ID, severity, status, remediation advice
- **Risk Scoring**: Weighted calculation (CRITICAL=25, HIGH=15, MEDIUM=8, LOW=3), capped at 100
- **Deduplication**: Same CVE + same target = update, not duplicate
- **Scan History**: Timestamp, duration, findings per scan (last 1000 records)

### Multi-Format Reporting

```bash
# Generate all formats simultaneously
python scanner.py -u https://grafana.example.com -o scan_results

# Creates:
#   scan_results.json   - Machine-readable JSON
#   scan_results.html   - Professional HTML report with severity badges
#   scan_results.csv    - Spreadsheet-compatible CSV
```

## Docker Usage

```bash
# Build
docker build -t grafana-scanner .

# Run single scan
docker run --rm grafana-scanner -u https://grafana.example.com

# Run with database (persistent)
docker run --rm -v $(pwd)/vulndb.json:/app/vulndb.json grafana-scanner -u https://grafana.example.com --db vulndb.json

# Run web dashboard
docker run --rm -p 8080:8080 -v $(pwd)/vulndb.json:/app/vulndb.json grafana-scanner --serve 8080 --host 0.0.0.0 --db vulndb.json
```

</br>

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for the full contribution workflow, including:

- Reporting bugs and suggesting enhancements
- Local development setup and test guidance
- Branching strategy and commit message conventions
- Pull request expectations and review checklist

We especially appreciate contributions in these areas:

- New CVE detection modules and vulnerability coverage
- False positive / false negative fixes
- Documentation updates and examples
- Test cases and reliability improvements
- Web dashboard UI and usability enhancements

Follow the steps in [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

</br>

## Credits

**Check out all these people who helped this repo:**

[![GitHub contributors](https://contrib.rocks/image?repo=amirrezafatemi/Grafana-Final-Scanner)](https://github.com/amirrezafatemi/Grafana-Final-Scanner/graphs/contributors)

**Thank you again for helping make Grafana Final Scanner better. Your time and effort are genuinely appreciated. ❤️**

</br>

## License

See [LICENSE](LICENSE) file for details.
