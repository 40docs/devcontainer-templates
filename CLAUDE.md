# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository provides devcontainer templates for the 40docs platform. It contains a comprehensive development environment configuration with 75+ tools and features for cloud-native development, security analysis, and DevOps workflows.

## Architecture

### Core Structure
- `src/devcontainer/`: Contains the main devcontainer template configuration
- `.devcontainer/devcontainer.json`: Comprehensive development container configuration
- `README.md`: Documentation with setup and usage instructions
- `NOTES.md`: Quick reference commands and troubleshooting

### Development Container Features
The devcontainer is built on Ubuntu 22.04 and includes:
- **Cloud Tools**: Azure CLI, AWS CLI, Google Cloud CLI, Terraform ecosystem
- **Container Tools**: Docker, Kubernetes (kubectl, helm, k9s), DevContainers CLI
- **Security Tools**: Lacework, DevSkim, Checkov, Terrascan, Trivy
- **Development Tools**: Multiple language runtimes (Python, Node.js, Go, Java, .NET, PHP)
- **Documentation Tools**: MkDocs ecosystem with multiple plugins
- **AI/ML Tools**: Ollama, CUDA support, various Python ML packages

## Common Development Commands

### DevContainer Operations
```bash
# Install DevContainers CLI globally
sudo npm install -g @devcontainers/cli

# Build multi-platform container (local development)
devcontainer build --workspace-folder . --platform linux/arm64,linux/amd64 --image-name ghcr.io/robinmordasiewicz/devcontainer:latest --output type=docker --no-cache true

# Create container from CLI
devcontainer up --workspace-folder ./

# Attach to running container
devcontainer exec --workspace-folder ./ /usr/bin/zsh

# Enable multi-architecture building
docker buildx create --name multiplatform --bootstrap --use
docker run --privileged --rm tonistiigi/binfmt --install all
```

### Container Registry Operations
```bash
# Authenticate to GitHub Container Registry
docker login ghcr.io

# Push built container
docker push ghcr.io/robinmordasiewicz/devcontainer:latest
```

### Environment Setup
```bash
# GitHub CLI authentication
gh auth login

# Terraform version management
tfenv install
tfenv use

# Clean Docker cache (troubleshooting)
docker system prune -a -f
```

## Development Workflow

### Pre-commit Hooks
The repository uses pre-commit hooks for quality assurance:
- JSON validation and formatting
- YAML validation
- Trailing whitespace removal
- GitHub workflow schema validation
- DevContainer schema validation

### CI/CD Pipeline
- **Trigger**: Changes to `src/devcontainer/.devcontainer/devcontainer.json`
- **Platform**: Custom runner (`template_runner`)
- **Build**: Multi-architecture support (linux/amd64)
- **Registry**: Pushes to `ghcr.io/40docs/devcontainer:latest`
- **Timeout**: 24 hours for complex builds

## Template Configuration

### Core Features Integration
The devcontainer template integrates multiple feature sources:
- **40docs Features**: Custom features from `ghcr.io/40docs/devcontainer-features/`
- **Community Features**: DevContainers community features
- **Third-party Features**: Specialized tools from various maintainers

### VSCode Customization
- **Extensions**: 75+ pre-installed extensions covering development, security, and productivity
- **Settings**: Optimized for development with proper shell profiles and terminal configuration
- **MCP Servers**: GitHub Copilot and Playwright integration

### Resource Requirements
- **GPU**: Optional support for CUDA workloads
- **Network**: Host networking for development services
- **Storage**: Extensive tooling requires significant disk space

## Platform-Specific Considerations

### macOS Development
- Disable Docker Desktop's containerd feature for compatibility
- Install Nerd fonts: `brew install --cask font-meslo-lg-nerd-font`
- Configure Ollama with proper network bindings using AppleScript

### Linux Development
- Enable QEMU for multi-architecture builds
- Ensure proper permissions for Docker socket
- Install BINFMT support for cross-platform building

## Key Integration Points

### 40docs Ecosystem
This template is designed to work seamlessly with:
- 40docs platform repositories
- Fortinet security tools and collections
- Ansible Galaxy collections for Fortinet products
- Kubernetes and cloud-native development workflows

### Security and Compliance
- Lacework integration for security scanning
- DevSkim for secure coding practices
- Multiple security scanning tools in CI/CD
- Secure credential handling with persistence features

## Troubleshooting

### Common Issues
- **Extension Loading**: Run `docker system prune -a -f` to clear cache
- **Multi-arch Builds**: Ensure QEMU is properly configured
- **Performance**: Template includes many features - consider customizing for specific use cases

### Development Tips
- Use tmux profile for persistent development sessions
- ZSH with extensive plugin configuration is the default shell
- MkDocs server runs on http://127.0.0.1:8000 for documentation preview

## Testing and Validation

### Schema Validation
- DevContainer configuration validates against official schema
- GitHub workflows validate against GitHub Actions schema
- Pre-commit hooks ensure code quality

### Build Validation
- Multi-platform build testing
- Container functionality verification
- Extension loading verification in CI/CD