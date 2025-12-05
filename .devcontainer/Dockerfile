FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu

# Hugo version
ARG HUGO_VERSION=0.152.2
# Docker injecta aquesta variable: amd64 o arm64
ARG TARGETARCH

RUN apt-get update && \
    apt-get install -y curl git ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Instal·lar Hugo Extended per a l'arquitectura correcta
RUN if [ "$TARGETARCH" = "arm64" ]; then \
    ARCH="arm64"; \
    else \
    ARCH="amd64"; \
    fi && \
    echo "Installing Hugo ${HUGO_VERSION} for arch ${ARCH}" && \
    curl -L "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-${ARCH}.tar.gz" \
    | tar -xz -C /usr/local/bin hugo

# Carpeta de treball per defecte (VS Code hi muntarà el projecte)
WORKDIR /workspaces