# GitHub Copilot Instructions

## Repository: devcontainer-templates

This repository provides comprehensive devcontainer templates with multi-architecture support and automated CI/CD.

## Architecture Overview

- **Template Structure**: Templates live in `src/[template-name]/.devcontainer/` with companion README.md and NOTES.md
- **Multi-arch Support**: Containers built for linux/amd64 with QEMU setup for cross-platform compatibility
- **Feature-Rich**: Heavily customized with 50+ devcontainer features from multiple registries (40docs, devcontainers, community)
- **Automated Workflows**: CI builds containers on devcontainer.json changes, auto-generates documentation

## Key Development Workflows

### Testing Templates Locally

```bash
# Install devcontainer CLI first
npm install -g @devcontainers/cli

# Build and test template
devcontainer build --workspace-folder src/devcontainer --image-name test-image

# Create container from CLI
devcontainer up --workspace-folder ./

# Attach to running container
devcontainer exec --workspace-folder ./ /usr/bin/zsh

```

### Multi-Architecture Building

```bash
# Setup buildx for multi-platform
docker buildx create --name multiplatform --bootstrap --use
docker run --privileged --rm tonistiigi/binfmt --install all

# Build with specific platforms
devcontainer build --workspace-folder . --platform linux/arm64,linux/amd64 --image-name ghcr.io/user/image:latest

```

## Project-Specific Conventions

### JSON Schema Validation

- All devcontainer.json files must validate against VS Code devcontainer schema
- Pre-commit hooks enforce JSON formatting and validation
- Use `check-jsonschema` for GitHub workflow validation

### Feature Organization Pattern

The main template uses a specific pattern for organizing features:

- **Core Infrastructure**: Base OS (Ubuntu 22.04), common-utils, docker-outside-of-docker
- **Development Tools**: Multiple language runtimes (Python 3.12, Node, Go, .NET 8.0, Java, PHP)
- **Cloud & DevOps**: AWS CLI, Azure CLI (with Bicep), GCP CLI, Kubernetes tools, Terraform ecosystem
- **Custom Features**: Heavily relies on `ghcr.io/40docs/devcontainer-features/*` for specialized tools

### Documentation Auto-Generation

- README.md files are auto-generated from template metadata
- Manual additions go in NOTES.md files
- Documentation workflow triggers on `src/**/*.json` changes

### Terminal & Shell Configuration

Templates are opinionated about shell setup:

- **Default Shell**: zsh with oh-my-zsh
- **Terminal Profiles**: bash, zsh, tmux, PowerShell configured
- **Prompt**: oh-my-posh with extensive plugin ecosystem

## Critical Integration Points

### CI/CD Pipeline Behavior

- **Trigger**: Only builds on `src/devcontainer/.devcontainer/devcontainer.json` changes
- **Disk Management**: Aggressively frees disk space before build (24-hour timeout)
- **Registry**: Pushes to `ghcr.io/40docs/devcontainer:latest`
- **Caching**: Uses previous image as cache-from

### Pre-commit Hooks

The `.pre-commit-config.yaml` is essential for maintaining quality:

- Validates JSON syntax and formatting
- Enforces devcontainer schema compliance
- Checks GitHub workflow syntax
- Auto-formats with prettier

### Extension Ecosystem

Templates include 70+ pre-installed VS Code extensions covering:

- Cloud platforms (AWS, Azure, GCP)
- Languages (Python, Go, .NET, Java, PHP)
- DevOps tools (Docker, Kubernetes, Terraform, Ansible)
- Security (DevSkim, Lacework)
- Productivity (GitHub Copilot, Live Share, GitLens)

## When Adding New Templates

1. Follow `src/[template-name]/.devcontainer/` structure
2. Include both README.md (auto-generated) and NOTES.md (manual)
3. Validate with: `pre-commit run --all-files`
4. Test build locally before PR
5. Ensure new templates include sample usage in documentation

## Common Troubleshooting

- **Build Failures**: Check disk space, GitHub runners have limited capacity
- **Extension Issues**: Run `docker system prune -a -f` to clear cache
- **Multi-arch**: Verify QEMU binfmt setup for cross-platform builds
- **Permission Issues**: Templates use git safe.directory and push.autoSetupRemote configs
