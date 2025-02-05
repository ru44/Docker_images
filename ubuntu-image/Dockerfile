FROM ubuntu:latest

# Install dependencies
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    wget \
    unzip \
    curl \
    git \
    nano \
    zsh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Oh My Zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Set the working directory
WORKDIR /app

# Add a volume for persistent storage
VOLUME ["/app/data"]

# Set the default shell to zsh
SHELL ["/bin/zsh", "-c"]
