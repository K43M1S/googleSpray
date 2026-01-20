# googleSpray

## Overview

`googleSpray` is a browser-based authentication assessment tool for Google platforms.
It supports two distinct operations:

* **User enumeration** against Google Workspace / Google Account login flows
* **Password spraying** using a single password across multiple accounts

The tool uses a real Chromium browser via `puppeteer-real-browser` to replicate
legitimate user behavior and evaluate authentication outcomes based on page state,
URL transitions, and response content.

Results are written in JSON Lines (JSONL) format to facilitate post-processing
and correlation.

---

## Features

* Real browser execution (Chromium)
* User enumeration without password submission
* Password spraying with controlled rate limiting
* Proxy support (with optional authentication)
* Headless and non-headless execution modes
* Retry logic and failure handling
* Screenshot capture for evidence and troubleshooting
* JSONL structured output

---

## Requirements

* Node.js ≥ 18
* Chromium-compatible browser (bundled or system-installed)
* Network access to Google authentication endpoints

---

## Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/RipFran/googleSpray.git
cd googleSpray
npm install
```

---

## Usage

The tool exposes two commands: `enum` and `spray`.

### User Enumeration

Enumerate valid Google accounts without attempting authentication:

```bash
node googleSpray.js enum -U users.txt -o enum_results.jsonl
```

Single user enumeration:

```bash
node googleSpray.js enum -u user@example.com
```

![alt text](media/enum.png)

### Password Spraying

Perform a password spray using a single password:

```bash
node googleSpray.js spray -U users.txt -p Winter2024! -o spray_results.jsonl
```

![alt text](media/spray.png)

---

## Common Options

| Option                   | Description                                        |
| ------------------------ | -------------------------------------------------- |
| `-U, --users <file>`     | File containing target email addresses             |
| `-u, --user <email>`     | Single target email                                |
| `-p, --password <value>` | Password string (spray only)                       |
| `--proxy <url>`          | Proxy configuration (`http://user:pass@host:port`) |
| `--headless`             | Run browser in headless mode                       |
| `-i, --interval <ms>`    | Delay between attempts (default: 5000)             |
| `-o, --output <file>`    | JSONL output file                                  |
| `--screenshots`          | Enable screenshot capture                          |
| `--screenshot-dir <dir>` | Screenshot output directory                        |
| `-v, --verbose`          | Verbose logging                                    |

---

## Output Format

Each execution generates one JSON object per target in JSONL format:

```json
{
  "status": "VALID_USER",
  "detail": "Account Exists",
  "elapsedMs": 3421,
  "module": "enum",
  "target": "user@example.com",
  "timestamp": "2026-01-20T08:31:12.123Z"
}
```

Possible states include:

* `VALID_USER`
* `INVALID_USER`
* `AUTH_FAILED`
* `SUCCESS`
* `UNKNOWN_ERROR`
* `CONNECTION_ERROR`

---

## Legal and Ethical Notice

This tool is intended exclusively for authorized security testing, internal audits,
and research activities conducted with explicit permission from the system owner.
Unauthorized use against third-party accounts or services may violate applicable
laws and terms of service.
