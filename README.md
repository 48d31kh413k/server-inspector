<div align="center">

# 🖥️ Server Inspector

**Comprehensive Linux server diagnostics — one script, zero dependencies.**

[![CI – ShellCheck Lint](https://github.com/48d31kh413k/server-inspector/actions/workflows/ci.yml/badge.svg)](https://github.com/48d31kh413k/server-inspector/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![ShellCheck](https://img.shields.io/badge/shellcheck-passing-brightgreen)](https://www.shellcheck.net/)
[![Bash](https://img.shields.io/badge/bash-5%2B-blue)](https://www.gnu.org/software/bash/)

</div>

---

## 📖 Overview

`server_info.sh` is a single-file Bash script that produces a **structured, colour-coded inspection report** for any Linux server.  
It is designed for DevOps engineers, SREs, and sysadmins who need a fast, repeatable way to audit a machine's state — during incident response, onboarding a new host, or routine health checks.

### What it reports

| Section | Details |
|---|---|
| 🏷️ Host & OS | Hostname, distro, kernel, architecture, uptime, timezone |
| ⚙️ CPU | Architecture, model, socket/core/thread count, hypervisor |
| 🧠 Memory | Total / used / free / swap (human-readable) |
| 💾 Disk | Block devices, filesystem usage (no tmpfs clutter) |
| 🌐 Network | Interfaces & IPs, routing table, DNS resolvers, public IP |
| 🔲 Virtualization | VM type (KVM, VMware, Docker, etc.) or physical server flag |
| 👤 Users & Security | Logged-in users, recent logins, sudo members, empty-password accounts |
| 🔧 Services | Top 20 running systemd services |
| 📊 Processes | Top 10 by CPU and top 10 by memory |
| 🐳 Docker | Engine version, container count, image count, `docker ps -a` |

---

## 🚀 Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/48d31kh413k/server-inspector.git
cd server-inspector

# 2. Make executable
chmod +x server_info.sh

# 3. Run
./server_info.sh
```

That's it — no pip install, no npm install, no configuration files.

---

## ⚙️ Usage

```
Usage:
  ./server_info.sh [OPTIONS]

Options:
  -o, --output FILE   Save the report to FILE instead of stdout
  --no-color          Disable coloured output (useful for log files / CI)
  -v, --version       Show version and exit
  -h, --help          Show this help message and exit
```

### Examples

```bash
# Print full report to terminal
./server_info.sh

# Save report to a timestamped file
./server_info.sh --output "report_$(hostname)_$(date +%F).txt"

# Plain text (no ANSI colour codes) — useful for piping / logging
./server_info.sh --no-color | tee /var/log/server_report.log

# Combine with sudo to unlock shadow-file security checks
sudo ./server_info.sh
```

---

## 🔄 Automation & Scheduling

### Cron — daily report at 06:00

```cron
0 6 * * * /opt/server-inspector/server_info.sh --no-color \
    --output /var/log/server-inspector/report_$(date +\%F).txt
```

### Ansible — run across a fleet

```yaml
- name: Run server inspector
  hosts: all
  tasks:
    - name: Copy script
      copy:
        src: server_info.sh
        dest: /usr/local/bin/server_info.sh
        mode: '0755'

    - name: Execute and save report
      shell: /usr/local/bin/server_info.sh --no-color
      register: report

    - name: Print report
      debug:
        var: report.stdout_lines
```

### Docker — inspect a container host

```bash
docker run --rm --privileged \
  -v /:/host:ro \
  ubuntu:22.04 bash -c \
  "apt-get update -qq && apt-get install -y procps iproute2 curl && \
   curl -fsSL https://raw.githubusercontent.com/48d31kh413k/server-inspector/main/server_info.sh | bash"
```

---

## 🛠️ Requirements

The script uses common Linux utilities that ship with virtually every distribution:

| Tool | Used for | Fallback |
|---|---|---|
| `bash` ≥ 4.0 | Script execution | — |
| `uname`, `hostname` | OS/kernel info | built-in |
| `lscpu` | CPU details | `nproc` |
| `free` | Memory usage | — |
| `lsblk`, `df` | Disk info | `df` alone |
| `ip` | Network info | `ifconfig` |
| `systemctl` | Services list | `service` |
| `ps` | Process list | — |
| `curl` / `wget` | Public IP lookup | skipped gracefully |
| `docker` | Container info | skipped gracefully |
| `dmidecode` | Hardware product name | skipped gracefully |

All sections degrade gracefully when a tool is absent — the script will never crash due to a missing optional dependency.

---

## 🧪 Running the Tests / Linter

```bash
# Syntax check only (no external tools needed)
bash -n server_info.sh

# Full ShellCheck lint
shellcheck server_info.sh

# Quick smoke test (--help and --version must not error)
bash server_info.sh --help
bash server_info.sh --version
```

CI runs these checks automatically on every push and pull request via [GitHub Actions](.github/workflows/ci.yml).

---

## 📂 Repository Structure

```
server-inspector/
├── server_info.sh        # Main inspection script
├── .github/
│   └── workflows/
│       └── ci.yml        # GitHub Actions CI (ShellCheck + smoke tests)
├── CHANGELOG.md          # Version history
├── CONTRIBUTING.md       # Contribution guide
├── LICENSE               # MIT License
└── README.md             # This file
```

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to:

- Report bugs
- Suggest enhancements
- Submit pull requests

---

## 📄 License

This project is released under the [MIT License](LICENSE).  
© 2026 server-inspector contributors

---

<div align="center">
  Made with ❤️ for the DevOps community
</div>
