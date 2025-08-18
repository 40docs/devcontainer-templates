# DevContainer Templates

Enterprise-grade development container templates for the 40docs platform, providing a comprehensive cloud-native development environment with extensive tooling for DevOps, security analysis, and modern application development.

## 🚀 Quick Start

### Use the Pre-built Container

Create a `.devcontainer/devcontainer.json` file in your project:

```json
{
  "name": "devops-toolkit",
  "image": "ghcr.io/40docs/devcontainer:latest",
  "initializeCommand": "docker pull ghcr.io/40docs/devcontainer:latest",
  "runArgs": ["--name=devcontainer", "--hostname=devcontainer"]
}
```

Then open your project in VS Code with the Dev Containers extension installed.

## 📦 What's Included

### Development Environments
- **Languages**: Python 3.12, Node.js 18, Go, Java, .NET 8.0, PHP
- **Frameworks**: FastAPI, Express, React, Vue, Angular support
- **Package Managers**: npm, pip, cargo, composer, maven

### Cloud & Infrastructure Tools
- **Cloud Platforms**: Azure CLI, AWS CLI, Google Cloud CLI
- **Infrastructure as Code**: Terraform (with tfenv), Ansible, Bicep
- **Container Orchestration**: Docker, Kubernetes (kubectl, helm, k9s, kubectx)
- **GitOps**: Flux, ArgoCD support

### Security & Compliance
- **Security Scanning**: Lacework CLI, DevSkim, Checkov, Terrascan, Trivy
- **Vulnerability Management**: Automated security scanning in CI/CD
- **Compliance**: OWASP, CIS benchmark tools

### AI/ML Development
- **GPU Support**: CUDA 12.4 toolkit, NVIDIA container runtime
- **AI Tools**: Ollama integration
- **ML Libraries**: TensorFlow, PyTorch, scikit-learn support

### Documentation & Productivity
- **Documentation**: MkDocs Material with 15+ plugins
- **Diagramming**: Mermaid, Draw.io integration
- **IDE**: 75+ pre-configured VS Code extensions
- **Terminal**: ZSH with Oh My Zsh, tmux for persistent sessions

## 🛠️ Development Setup

### Prerequisites

Install the DevContainers CLI:
```bash
sudo npm install -g @devcontainers/cli
```

### Building the Container

#### Enable Multi-Architecture Support
```bash
# Create multi-platform builder
docker buildx create --name multiplatform --bootstrap --use

# Enable emulation for cross-platform builds
docker run --privileged --rm tonistiigi/binfmt --install all
```

#### Build for Multiple Architectures
```bash
devcontainer build --workspace-folder . \
  --platform linux/arm64,linux/amd64 \
  --image-name ghcr.io/40docs/devcontainer:latest \
  --output type=docker \
  --no-cache true
```

#### Push to Registry
```bash
# Authenticate to GitHub Container Registry
docker login ghcr.io

# Push the container
docker push ghcr.io/40docs/devcontainer:latest
```

## 💻 Platform-Specific Configuration

### macOS Setup

1. **Install Nerd Fonts** for optimal terminal experience:
```bash
brew tap homebrew/cask-fonts
brew install --cask font-meslo-lg-nerd-font
```

2. **Configure VS Code**:
   - Open Settings → Search "terminal.integrated.fontFamily"
   - Set to "MesloLGS NF"

3. **Docker Desktop Settings**:
   - Disable "Use containerd for pulling and storing images"
   - Disable Rosetta emulation
   - Find in: Settings → Features in development

4. **Ollama Configuration** (for AI development):
```applescript
do shell script "launchctl setenv OLLAMA_ORIGINS '*'"
do shell script "launchctl setenv OLLAMA_HOST \"0.0.0.0\""
tell application "Ollama" to run
```
Save as Application and add to login items.

### Linux Setup

Ensure QEMU support for multi-architecture builds:
```bash
docker run --privileged --rm tonistiigi/binfmt --install all
docker info -f '{{ .DriverStatus }}'
```

## 🔧 Usage

### CLI Operations

```bash
# Create and start container
devcontainer up --workspace-folder ./

# Attach to running container
devcontainer exec --workspace-folder ./ /usr/bin/zsh

# Access specific services
# MkDocs: http://127.0.0.1:8000
```

### Development Workflows

```bash
# GitHub authentication
gh auth login

# Terraform management
tfenv install
tfenv use

# Clear Docker cache (troubleshooting)
docker system prune -a -f
```

## 🤖 AI & Automation Features

### MCP Server Integration
- GitHub Copilot MCP server for AI assistance
- Playwright server for browser automation and testing

### Persistent Configurations
- Shell history and preferences
- CLI authentication tokens (GitHub, Azure, AWS, GCloud)
- Git configuration and SSH keys

## 📋 CI/CD Integration

The container is automatically built and published when:
- Changes are made to `src/devcontainer/.devcontainer/devcontainer.json`
- Manual workflow dispatch is triggered
- Repository dispatch events occur from feature updates

### Build Pipeline
- **Runner**: Custom GitHub runner (`template_runner`)
- **Timeout**: 24 hours for complex multi-architecture builds
- **Registry**: Publishes to `ghcr.io/40docs/devcontainer:latest`

## 🐛 Troubleshooting

### Common Issues

**Extensions not loading:**
```bash
docker system prune -a -f
```

**Multi-architecture build failures:**
```bash
# Verify QEMU installation
docker buildx inspect multiplatform

# Reinstall BINFMT support
docker run --privileged --rm tonistiigi/binfmt --install all
```

**Performance optimization:**
- The container includes extensive tooling
- Consider customizing `devcontainer.json` to disable unused features
- Use resource limits in Docker settings for constrained environments

## 📝 Repository Structure

```
devcontainer-templates/
├── src/
│   └── devcontainer/
│       ├── .devcontainer/
│       │   └── devcontainer.json    # Main configuration
│       ├── README.md                 # Template documentation
│       ├── NOTES.md                  # Quick reference
│       └── docker-desktop-settings.png
├── .github/
│   └── workflows/
│       └── devcontainer.yml          # CI/CD pipeline
├── LICENSE                           # MIT License
├── CLAUDE.md                         # AI assistant guidelines
└── README.md                         # This file
```

## 🤝 Contributing

This repository uses pre-commit hooks for quality assurance:
- JSON validation and formatting
- YAML schema validation
- DevContainer configuration validation

Install pre-commit hooks:
```bash
pre-commit install
```

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

## 🔗 Related Resources

- [VS Code Dev Containers Documentation](https://code.visualstudio.com/docs/devcontainers/containers)
- [40docs Platform](https://github.com/40docs)
- [DevContainer Features](https://github.com/40docs/devcontainer-features)

## 🏷️ Version

Current image: `ghcr.io/40docs/devcontainer:latest`

---

*Part of the 40docs Documentation as Code ecosystem - Managing 25+ interconnected repositories through automation and best practices.*