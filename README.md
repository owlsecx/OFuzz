# 🦉 OFuzz

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux%20%2F%20Windows-informational?style=flat-square&logo=linux&logoColor=white&color=0a0c10"/>
  <img src="https://img.shields.io/badge/Category-OWeb%20%2F%20Reconnaissance-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/No%20Dependencies-Stdlib%20Only-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-1.0-cyan?style=flat-square"/>
</p>

> **OFuzz** is a multi-mode web fuzzer — directory brute-force, file and extension discovery, hidden GET/POST parameter detection, DNS subdomain enumeration, and virtual host fuzzing, with built-in wordlists, cookie authentication support, and JSON export.

---

> ⚠️ **AUTHORISED SECURITY TESTING ONLY** — Use only on systems you own or have explicit written consent to test.

---

## 📌 Overview

OFuzz runs multi-threaded HTTP probes to discover hidden resources on web applications. All four fuzz modes share a common engine that displays live progress, colour-coded status codes, and per-scan summaries. Results can be exported as JSON after each scan or as a full session export.

---

## 🖥️ Modules

| # | Module | Description |
|---|--------|-------------|
| **[1] Directory Fuzzer** | Brute-forces URL paths from the wordlist — reports status code, content size, and redirects |
| **[2] File Fuzzer** | Combines filenames × extensions to discover files — flags sensitive extensions with `[!]` |
| **[3] Parameter Fuzzer** | Sends each parameter name with a test value and compares response size/status against a baseline — reports parameters that cause a different response |
| **[4] Subdomain Fuzzer** | DNS resolution mode — resolves `word.domain` and reports IP · VHost mode — sends Host header variants and detects responses differing from baseline |
| **[5] Wordlist Manager** | View built-in wordlist sizes and preview first entries |

---

## 📚 Built-in Wordlists

| Wordlist | Entries | Coverage |
|----------|---------|----------|
| **Dirs** | ~120 | Admin panels, API endpoints, dev paths, CMS paths, config files, secret directories, CI/CD tools, monitoring endpoints, `.git`, `.env` |
| **Files** | ~60 | Config files, backup files, credential files, log files, CI files, SSH keys, environment files (`.env.*`) |
| **Params** | ~80 | Common GET/POST parameter names: `id`, `user`, `file`, `cmd`, `redirect`, `token`, `debug`, `include`, `upload`, `callback` |
| **Subdomains** | ~100 | Mail, dev, staging, API, admin, CDN, git, CI/CD, monitoring, database, VPN, geo regions |

Custom wordlists can be provided as `.txt` files (one entry per line). Custom lists are combined with built-in by default.

---

## 🔐 HTTP Engine Features

| Feature | Detail |
|---------|--------|
| **Threading** | Default 10 threads · configurable per scan |
| **Delay** | Configurable request delay (seconds) to avoid rate-limiting |
| **Cookie auth** | Pass session cookie for authenticated scanning |
| **TLS** | SSL verification disabled — scans self-signed cert targets |
| **No-redirect** | Redirects are captured and reported, not followed |
| **User-Agent** | Chrome 120 UA string |

---

## 📊 Status Code Legend

| Code | Meaning |
|------|---------|
| **200** | Found — content returned |
| **301 / 302 / 307 / 308** | Redirect — destination reported |
| **401** | Unauthorised — authentication required |
| **403** | Forbidden — path exists but access denied |
| **404** | Not Found — filtered from results by default |
| **500** | Server Error — may indicate discovery |

---

## 🗂️ Sensitive File Flagging

The File Fuzzer automatically flags discovered files with high-risk extensions:

`.bak` · `.old` · `.sql` · `.env` · `.config` · `.conf` · `.cfg` · `.zip` · `.tar` · `.gz`

---

## 💻 CLI Mode

OFuzz supports non-interactive command-line usage:

```bash
# Directory fuzz
./OFuzz -u https://target.com -d -w wordlist.txt

# File fuzz with extensions
./OFuzz -u https://target.com -f -x .php,.bak,.env

# Parameter fuzz
./OFuzz -u https://target.com/search -p -w params.txt

# Subdomain enumeration
./OFuzz -s target.com -w subdomains.txt

# With cookie, threads, delay, and output
./OFuzz -u https://target.com -d -c "session=abc123" -t 20 -D 0.5 -o results.json

# Filter specific status codes
./OFuzz -u https://target.com -d -fc 404,400
```

### CLI Flags

| Flag | Description | Default |
|------|-------------|---------|
| `-u` / `--url` | Target URL | — |
| `-d` / `--dirs` | Directory fuzz mode | — |
| `-f` / `--files` | File fuzz mode | — |
| `-p` / `--params` | Parameter fuzz mode | — |
| `-s` / `--sub` | Subdomain fuzz (domain, not URL) | — |
| `-w` / `--wordlist` | Custom wordlist file | built-in |
| `-x` / `--exts` | File extensions (comma-separated) | built-in |
| `-t` / `--threads` | Thread count | `10` |
| `-D` / `--delay` | Delay between requests (seconds) | `0.0` |
| `-c` / `--cookie` | Cookie header value | — |
| `-fc` / `--filter` | Status codes to hide | `404` |
| `-o` / `--output` | Save JSON report to file | — |

---

## 📤 Export

JSON reports saved as `ofuzz_<type>_YYYYMMDD_HHMMSS.json` after each scan, or `ofuzz_session_*.json` for the full session export via **[E] Export Session**.

---

## ⚙️ Requirements

- **Linux or Windows**
- **No Python installation needed** — runs as a standalone executable
- **No external dependencies** — stdlib only

---

## 🚀 Usage

```bash
# Interactive menu
./OFuzz

# Direct CLI
./OFuzz -u https://target.com -d
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
