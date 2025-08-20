# Choose Ubuntu base aligned with devcontainer images
ARG UBUNTU_VARIANT=22.04
FROM mcr.microsoft.com/devcontainers/base:ubuntu-${UBUNTU_VARIANT}

ENV DEBIAN_FRONTEND=noninteractive
# Detect codename (jammy/noble), arch (amd64/arm64), and version string (22.04/24.04)
RUN set -eux; \
    . /etc/os-release; \
    echo "VERSION_CODENAME=${VERSION_CODENAME}" > /etc/os-release-derived; \
    echo "VERSION_ID=${VERSION_ID}"           >> /etc/os-release-derived; \
    dpkg --print-architecture > /etc/dpkg-arch

# Base deps for HTTPS repos & PPAs
RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates curl gnupg software-properties-common lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Enable universe/multiverse
RUN add-apt-repository -y universe \
 && add-apt-repository -y multiverse

# PPAs (authd, dotnet backports, ansible, nvtop, graphics-drivers)
RUN add-apt-repository -y ppa:ubuntu-enterprise-desktop/authd \
 && add-apt-repository -y ppa:dotnet/backports \
 && add-apt-repository -y ppa:ansible/ansible \
 && add-apt-repository -y ppa:quentiumyt/nvtop \
 && add-apt-repository -y ppa:graphics-drivers/ppa

# Keyrings directory
RUN install -d -m 0755 /etc/apt/keyrings

# Helper variables for later RUNs
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Read derived values each time
#   CODENAME: jammy/noble
#   VERSTR:   22.04/24.04
#   ARCH:     amd64/arm64
RUN CODENAME="$(. /etc/os-release-derived; echo ${VERSION_CODENAME})"; \
    VERSTR="$(. /etc/os-release-derived; echo ${VERSION_ID})"; \
    ARCH="$(cat /etc/dpkg-arch)"; \
\
# --- Trivy ---
    curl -fsSL https://get.trivy.dev/deb/public.key \
      | gpg --dearmor -o /etc/apt/keyrings/trivy.gpg; \
    chmod a+r /etc/apt/keyrings/trivy.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/trivy.gpg] https://get.trivy.dev/deb generic main" \
      > /etc/apt/sources.list.d/trivy.list; \
\
# --- HashiCorp ---
    curl -fsSL https://apt.releases.hashicorp.com/gpg \
      | gpg --dearmor -o /etc/apt/keyrings/hashicorp.gpg; \
    chmod a+r /etc/apt/keyrings/hashicorp.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/hashicorp.gpg] https://apt.releases.hashicorp.com ${CODENAME} main" \
      > /etc/apt/sources.list.d/hashicorp.list; \
\
# --- GitHub CLI ---
    curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg \
      -o /etc/apt/keyrings/githubcli-archive-keyring.gpg; \
    chmod a+r /etc/apt/keyrings/githubcli-archive-keyring.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
      > /etc/apt/sources.list.d/github-cli.list; \
\
# --- Google Cloud SDK ---
    curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg \
      | gpg --dearmor -o /etc/apt/keyrings/google-cloud.gpg; \
    chmod a+r /etc/apt/keyrings/google-cloud.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/google-cloud.gpg] http://packages.cloud.google.com/apt cloud-sdk main" \
      > /etc/apt/sources.list.d/google-cloud-sdk.list; \
\
# --- Google Chrome ---
    curl -fsSL https://dl.google.com/linux/linux_signing_key.pub \
      | gpg --dearmor -o /etc/apt/keyrings/google-chrome.gpg; \
    chmod a+r /etc/apt/keyrings/google-chrome.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" \
      > /etc/apt/sources.list.d/google-chrome.list; \
\
# --- Docker CE ---
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
      | gpg --dearmor -o /etc/apt/keyrings/docker.gpg; \
    chmod a+r /etc/apt/keyrings/docker.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu ${CODENAME} stable" \
      > /etc/apt/sources.list.d/docker.list; \
\
# --- NVIDIA container toolkit (libnvidia-container) ---
    curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey \
      | gpg --dearmor -o /etc/apt/keyrings/nvidia-container-toolkit.gpg; \
    chmod a+r /etc/apt/keyrings/nvidia-container-toolkit.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/nvidia-container-toolkit.gpg] https://nvidia.github.io/libnvidia-container/stable/deb/${ARCH} /" \
      > /etc/apt/sources.list.d/nvidia-container-toolkit.list; \
\
# --- Azure CLI ---
    curl -fsSL https://packages.microsoft.com/keys/microsoft.asc \
      | gpg --dearmor -o /etc/apt/keyrings/microsoft.gpg; \
    chmod a+r /etc/apt/keyrings/microsoft.gpg; \
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/azure-cli/ ${CODENAME} main" \
      > /etc/apt/sources.list.d/azure-cli.list; \
\
# --- VS Code ---
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/vscode stable main" \
      > /etc/apt/sources.list.d/vscode.list; \
\
# --- PowerShell (versioned path by Ubuntu version) ---
    echo "deb [arch=${ARCH} signed-by=/etc/apt/keyrings/microsoft.gpg] https://packages.microsoft.com/ubuntu/${VERSTR}/prod ${CODENAME} main" \
      > /etc/apt/sources.list.d/microsoft-powershell.list

# Final refresh (repos now available for later install layers)
RUN apt-get update

