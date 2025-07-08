# env2llm Implementation Guide

## Overview
This guide provides step-by-step instructions for implementing `env2llm`, a command-line tool that profiles a Python developer's environment for LLM context. The tool outputs a text file containing relevant system, Python, Docker, and Kubernetes information optimized for proof-of-concept development.

## Core Principles
1. **Fail gracefully** - Missing tools should not crash the script
2. **Security first** - Never expose sensitive information (passwords, tokens, keys)
3. **Speed matters** - Execute quickly, skip expensive operations
4. **Signal over noise** - Only include information relevant for PoC development
5. **Human readable** - Output should be clear and well-organized

## Implementation Steps

### Step 1: Script Structure
Create a bash script with the following structure:

```bash
#!/usr/bin/env bash
# env2llm - Development environment profiler for LLM context
# Version: 1.0.0

# 1. Script configuration section
# 2. Utility functions
# 3. Information gathering functions
# 4. Main execution logic
# 5. Help/usage information
```

### Step 2: Core Utility Functions
Implement these essential utility functions:

1. **`command_exists()`** - Check if a command is available
   - Input: command name
   - Output: return 0 if exists, 1 if not
   - Purpose: Prevent errors from missing tools

2. **`print_section()`** - Format section headers
   - Input: section title
   - Output: Formatted header with visual separator
   - Purpose: Organize output into readable sections

3. **`safe_exec()`** - Execute commands with error handling
   - Input: command and arguments
   - Output: Command output or error message
   - Purpose: Gracefully handle command failures

4. **`check_port()`** - Check if a port is in use
   - Input: port number
   - Output: Status message
   - Purpose: Identify potential conflicts

### Step 3: Information Gathering Functions
Create functions for each major section:

#### System Information Function
```
gather_system_info()
- OS type and version
- Memory (total GB)
- Disk space (available GB)
- CPU architecture
- Skip: detailed hardware specs, load averages
```

#### Shell Environment Function
```
gather_shell_info()
- Current shell ($SHELL)
- Working directory (pwd)
- Key env vars: PATH (first 5 entries), PYTHONPATH, VIRTUAL_ENV
- Shell framework detection (Oh My Zsh, etc.)
- Skip: aliases, functions, full PATH
```

#### Python Environment Function
```
gather_python_info()
- Python version (python3 --version)
- pip version
- Active virtual environment
- Check for: requirements.txt, pyproject.toml, Pipfile
- List of installed packages (count only, not full list)
- Skip: all packages details, multiple Python versions
```

#### Docker Environment Function
```
gather_docker_info()
- Docker version
- Docker daemon status
- Docker Compose version
- Running containers (count and names)
- Check for: Dockerfile, docker-compose.yml, .dockerignore
- Skip: all images, detailed container info
```

#### Kubernetes Environment Function
```
gather_kubernetes_info()
- Local K8s type (check for minikube, kind, docker-desktop)
- kubectl version
- Current context and namespace
- Cluster accessibility (can connect to API)
- Check for: k8s manifests (*.yaml in k8s/ or manifests/)
- Skip: all resources, detailed cluster info
```

#### Project Context Function
```
gather_project_info()
- Git status (branch, clean/dirty)
- Project structure (top-level directories)
- Entry point detection (app.py, main.py, manage.py)
- Config files present (.env, config.py)
- Skip: file contents, git history
```

#### Network/Ports Function
```
gather_network_info()
- Check common ports: 3000, 5000, 8000, 8080
- Internet connectivity (can reach github.com)
- Proxy settings (if set)
- Skip: detailed network configuration
```

### Step 4: Output Formatting
Structure the output for optimal LLM consumption:

```
=== env2llm Profile ===
Generated: [timestamp]
Purpose: Python/Docker/Kubernetes PoC Development

[SYSTEM]
- OS: macOS 13.0
- Memory: 16 GB
- Disk Available: 125 GB
- Architecture: arm64

[SHELL]
- Shell: /bin/zsh
- Working Dir: /Users/dev/projects/myapp
- Virtual Env: myapp-env
- Framework: Oh My Zsh

[Continue for each section...]
```

### Step 5: Error Handling Strategy
1. Never use `set -e` (fail on error) - continue gathering what's available
2. Wrap each major section in error handling
3. Provide meaningful messages for missing tools
4. Use default values where appropriate

### Step 6: Performance Optimizations
1. Use command substitution efficiently
2. Avoid recursive file searches
3. Limit output from commands (use head, count)
4. Skip expensive operations (full package lists, image scanning)
5. Cache results that might be used multiple times

### Step 7: Security Considerations
1. Never output environment variables that might contain secrets
2. Mask sensitive parts of git remotes (tokens in URLs)
3. Skip .env file contents
4. Don't show full paths for security-sensitive locations
5. Redact user-specific information where unnecessary

### Step 8: Help/Usage Implementation
```bash
if [[ "$1" == "--help" ]] || [[ "$1" == "-h" ]]; then
    cat << EOF
env2llm - Development Environment Profiler for LLM Context

Usage: env2llm [--help]

Description:
  Generates a comprehensive profile of your Python/Docker/Kubernetes
  development environment optimized for sharing with LLMs.

Output:
  Writes to stdout. Redirect to save:
  env2llm > profile.txt

Requirements:
  - Bash 4.0+
  - Python 3.x development environment
  - Docker (optional)
  - Kubernetes (optional)

EOF
    exit 0
fi
```

### Step 9: Testing Checklist
Before considering the implementation complete:

1. **Test with minimal environment** - Only Python installed
2. **Test with full environment** - Python, Docker, K8s all present
3. **Test with missing components** - No Docker, no K8s
4. **Test in different directories** - Git repo, non-git directory
5. **Test with active virtual environment** - And without
6. **Test execution time** - Should complete in under 5 seconds
7. **Test output size** - Should be under 1000 lines

### Step 10: Example Implementation Pattern
Here's a pattern for implementing each section:

```bash
gather_section_name() {
    echo "[SECTION_NAME]"
    
    # Check for tool availability
    if command_exists tool_name; then
        # Gather information with error handling
        info=$(tool_name --version 2>/dev/null || echo "Error getting version")
        echo "- Tool: $info"
    else
        echo "- Tool: Not installed"
    fi
    
    # Check for configuration files
    if [[ -f "config_file" ]]; then
        echo "- Config: config_file found"
    fi
    
    echo ""  # Empty line between sections
}
```

## Quality Checklist
- [ ] Executes without errors on macOS and Linux
- [ ] Completes in under 5 seconds
- [ ] Output is under 1000 lines
- [ ] No sensitive information exposed
- [ ] Handles all missing tools gracefully
- [ ] Output is well-formatted and readable
- [ ] Includes timestamp and version
- [ ] Help text is clear and accurate

## Future Enhancements (Not for v1)
- JSON output format option
- Verbosity levels
- Custom checks via plugins
- Configuration file support
- Update checking