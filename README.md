# DevContainer Templates

Comprehensive development container templates for the 40docs platform, providing a fully-featured development environment with 75+ tools and integrations for cloud-native development, security analysis, and DevOps workflows.

## Overview

This repository contains devcontainer templates built on Ubuntu 22.04 with extensive tooling for:

- **Cloud Development**: Azure, AWS, Google Cloud integration
- **Container & Kubernetes**: Docker, kubectl, helm, k9s, and related tools  
- **Security Analysis**: Lacework, DevSkim, Checkov, Terrascan, Trivy
- **Infrastructure as Code**: Terraform ecosystem, Ansible, Bicep
- **Development Languages**: Python, Node.js, Go, Java, .NET, PHP
- **Documentation**: MkDocs with extensive plugin ecosystem
- **AI/ML Development**: CUDA support, Ollama, Python ML packages

## Quick Start

### Using the Pre-built Container

Create a `.devcontainer/devcontainer.json` file in your project:

```json
{
  "name": "devops-toolkit",
  "image": "ghcr.io/40docs/devcontainer:latest",
  "initializeCommand": "docker pull ghcr.io/40docs/devcontainer:latest",
  "runArgs": ["--name=devcontainer", "--hostname=devcontainer"]
}
```

### Using DevContainers CLI

```bash
# Create container from CLI
devcontainer up --workspace-folder ./

# Attach to running container
devcontainer exec --workspace-folder ./ /usr/bin/zsh
```

## Development Setup

### Prerequisites

Install DevContainers CLI:
```bash
sudo npm install -g @devcontainers/cli
```

### Multi-Architecture Building

Enable multi-architecture support:
```bash
# Create multi-platform builder
docker buildx create --name multiplatform --bootstrap --use
docker buildx ls

# Enable emulation for cross-platform builds
docker run --privileged --rm tonistiigi/binfmt --install all
```

### Building the Container

Build multi-platform container:
```bash
devcontainer build --workspace-folder . \
  --platform linux/arm64,linux/amd64 \
  --image-name ghcr.io/40docs/devcontainer:latest \
  --output type=docker \
  --no-cache true
```

### Publishing to Registry

```bash
# Authenticate to GitHub Container Registry
docker login ghcr.io

# Push the container
docker push ghcr.io/40docs/devcontainer:latest
```

## Included Tools & Features

### Cloud & Infrastructure
- Azure CLI with Bicep support
- AWS CLI with session persistence
- Google Cloud CLI with persistence
- Terraform with tfenv version management
- Ansible with Fortinet collections
- Kubernetes tools (kubectl, helm, k9s, kubectx)

### Security & Compliance
- Lacework CLI and extensible reporting
- DevSkim security analyzer
- Checkov IaC security scanning
- Terrascan policy scanning
- Trivy vulnerability scanner

### Development Languages & Runtimes
- Python 3.12 with extensive ML packages
- Node.js 18 with global development tools
- Go with package installation tools
- Java development kit
- .NET 8.0 with C# DevKit
- PHP development environment

### Documentation & Publishing
- MkDocs Material with 15+ plugins
- Markdown tools and linting
- Draw.io integration
- Mermaid diagram support

### AI & Machine Learning
- CUDA 12.4 support with toolkit
- NVIDIA container runtime
- Ollama integration
- Python ML packages (FastAPI, Pydantic, etc.)

## Platform-Specific Setup

### macOS Configuration

Install Nerd fonts for optimal terminal experience:
```bash
brew tap homebrew/cask-fonts
brew install --cask font-meslo-lg-nerd-font
```

Configure Docker Desktop:
1. Disable "Use containerd for pulling and storing images"
2. Disable Rosetta emulation
3. Find settings in Docker Desktop > Settings > Features in development

### Ollama Setup (macOS)

Configure Ollama for container access:
```applescript
do shell script "launchctl setenv OLLAMA_ORIGINS '*'"
do shell script "launchctl setenv OLLAMA_HOST \"0.0.0.0\""
tell application "Ollama" to run
```

Save as an Application and add to login items.

### Linux Configuration

Ensure proper Docker buildx support:
```bash
docker run --privileged --rm tonistiigi/binfmt --install all
docker info -f '{{ .DriverStatus }}'
```

## VSCode Integration

### Required Extensions

The container includes 75+ pre-installed extensions covering:
- Language support (Python, Go, Java, C#, PHP)
- Cloud tools (Azure, AWS, Kubernetes)
- Security scanning (Lacework, DevSkim)
- Documentation (Markdown, Mermaid)
- Development productivity (GitLens, Copilot)

### Terminal Configuration

Configured terminal profiles:
- **zsh**: Default shell with Oh My Zsh and plugins
- **bash**: Standard bash shell
- **tmux**: Persistent session management
- **pwsh**: PowerShell 7.4.2

## Environment Features

### MCP Server Integration
- GitHub Copilot MCP server
- Playwright testing server

### Development Services
- MkDocs development server: http://127.0.0.1:8000
- Automatic port forwarding for development services
- PostgreSQL database for development

### Persistence Features
- Shell history persistence
- CLI authentication persistence (gh, az, aws, gcloud)
- Git configuration preservation

## Troubleshooting

### Common Issues

**Extensions not loading:**
```bash
docker system prune -a -f
```

**Multi-architecture build issues:**
Ensure QEMU and buildx are properly configured:
```bash
docker buildx inspect multiplatform
docker run --privileged --rm tonistiigi/binfmt --install all
```

**Performance issues:**
The container includes extensive tooling. Consider customizing the devcontainer.json to disable unused features for better performance.

## Contributing

This repository uses pre-commit hooks for quality assurance:
- JSON validation and formatting
- YAML schema validation
- DevContainer configuration validation

Install pre-commit hooks:
```bash
pre-commit install
```

## CI/CD

The container is automatically built and published on:
- Changes to `src/devcontainer/.devcontainer/devcontainer.json`
- Manual workflow dispatch
- Repository dispatch events from feature updates

Build runs on custom GitHub runners with 24-hour timeout for complex multi-architecture builds.
