FROM ubuntu:latest

# Install necessary packages
RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    git \
    nano \
    zsh \
    sudo \
    conntrack \
    socat \
    iptables \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Docker
RUN install -m 0755 -d /etc/apt/keyrings \
    && curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc \
    && chmod a+r /etc/apt/keyrings/docker.asc \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" > /etc/apt/sources.list.d/docker.list \
    && apt-get update \
    && apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin \
    && rm -rf /var/lib/apt/lists/*

# Install Minikube
RUN ARCH=$(dpkg --print-architecture) && \
    if [ "$ARCH" = "amd64" ]; then \
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb; \
    elif [ "$ARCH" = "arm64" ]; then \
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_arm64.deb; \
    else \
    echo "Unsupported architecture: $ARCH"; exit 1; \
    fi && \
    dpkg -i minikube_latest_*.deb && \
    rm minikube_latest_*.deb

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/$(dpkg --print-architecture)/kubectl" \
    && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Oh My Zsh (Non-Interactive)
RUN RUNZSH=no sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Set non-root user
RUN useradd -m -s /bin/zsh minikube-user && echo "minikube-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
USER minikube-user
WORKDIR /home/minikube-user

# Ensure Minikube uses the host's Docker
RUN mkdir -p ~/.minikube \
    && echo '{ "driver": "docker" }' > ~/.minikube/config.json

# Default command to start Minikube
CMD ["minikube", "start", "--driver=docker"]