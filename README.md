# env2llm

A fast, lightweight bash script that profiles your Python development environment and outputs human-readable text optimized for sharing with LLMs (Large Language Models).

## Overview

`env2llm` helps developers quickly share their development environment context with LLMs for troubleshooting, setup assistance, and proof-of-concept development. It gathers relevant information about your system, Python environment, Docker, Kubernetes, project structure, and network configuration while keeping sensitive information secure.

## Features

- **Fast execution** - Completes in under 5 seconds
- **Graceful degradation** - Works even when tools are missing
- **Security-first** - Never exposes sensitive information (passwords, tokens, keys)
- **Cross-platform** - Works on macOS and Linux
- **LLM-optimized** - Output format designed for AI context sharing
- **Lightweight** - Single bash script with no dependencies

## Installation

### Option 1: Direct Download
```bash
curl -o env2llm https://raw.githubusercontent.com/your-username/env2llm/main/env2llm
chmod +x env2llm
```

### Option 2: Clone Repository
```bash
git clone https://github.com/your-username/env2llm.git
cd env2llm
chmod +x env2llm
```

### Option 3: Add to PATH
```bash
# After downloading, copy to a directory in your PATH
sudo cp env2llm /usr/local/bin/
```

## Usage

### Basic Usage
```bash
# Output to terminal
./env2llm

# Save to file
./env2llm > my-environment.txt

# If installed in PATH
env2llm > profile.txt
```

### Help
```bash
./env2llm --help
```

## Sample Output

```
=== env2llm Profile ===
Generated: Tue Jul 8 15:46:02 MDT 2025
Purpose: Python/Docker/Kubernetes PoC Development

[SYSTEM]
- OS: macOS 14.1
- Memory: 16 GB
- Disk Available: 58Gi
- Architecture: arm64

[SHELL]
- Shell: /bin/zsh
- Working Dir: /Users/dev/myproject
- Virtual Env: myproject-env
- Framework: Oh My Zsh

[PYTHON]
- Python: Python 3.11.5
- pip: 23.2.1
- Installed packages: 42
- Project files: requirements.txt pyproject.toml

[DOCKER]
- Docker: 24.0.6
- Daemon: Running
- Running containers: 3
- Container names: web-app, redis, postgres
- Docker Compose: 2.21.0
- Docker files: Dockerfile docker-compose.yml

[KUBERNETES]
- kubectl: v1.28.2
- Current context: minikube
- Current namespace: default
- Cluster access: Connected
- Local K8s: minikube
- K8s manifests: 5 found

[PROJECT]
- Git branch: main
- Git status: Clean
- Git remote: https://github.com/user/project.git
- Top-level directories: 8
- Key directories: src tests docs
- Entry points: app.py
- Config files: .env config.py

[NETWORK]
Port 3000: Available
Port 5000: In use
Port 8000: Available
Port 8080: In use
- Internet: Connected (github.com reachable)
- Proxy: Not configured

=== End Profile ===
```

## What's Included

### System Information
- Operating system and version
- Available memory and disk space
- CPU architecture

### Shell Environment
- Current shell and working directory
- Virtual environment status
- Key environment variables (PATH, PYTHONPATH)
- Shell framework detection (Oh My Zsh, etc.)

### Python Environment
- Python and pip versions
- Package count
- Project files (requirements.txt, pyproject.toml, etc.)

### Docker Environment
- Docker and Docker Compose versions
- Daemon status and running containers
- Docker-related files in current directory

### Kubernetes Environment
- kubectl version and cluster connectivity
- Current context and namespace
- Local Kubernetes type detection
- Manifest file detection

### Project Context
- Git branch and status
- Project structure overview
- Entry point detection
- Configuration files

### Network Information
- Common development port availability
- Internet connectivity
- Proxy configuration

## Security

`env2llm` is designed with security in mind:

- **No sensitive data exposure** - Passwords, tokens, and keys are never included
- **Sanitized URLs** - Git remotes are cleaned of embedded credentials
- **No file contents** - Only checks for file existence, never reads contents
- **Limited path disclosure** - Avoids exposing security-sensitive locations

## Requirements

- **Bash 4.0+** (available on all modern systems)
- **Python 3.x** development environment
- **Docker** (optional)
- **Kubernetes** (optional)

## Use Cases

### For Developers
- Share environment context with team members
- Document development setup for onboarding
- Troubleshoot environment issues with AI assistance
- Quick environment audits

### For LLM Interactions
- Provide comprehensive context for coding assistance
- Enable environment-specific recommendations
- Support for containerized development workflows
- Help with deployment and configuration issues

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Version

Current version: 1.0.0