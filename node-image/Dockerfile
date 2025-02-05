FROM node:23-alpine3.20

# Install dependencies

RUN apk update && apk add --no-cache \
    wget \
    unzip \
    curl \
    git \
    nano \
    zsh

# Install Oh My Zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Add a volume for persistent storage
VOLUME ["/app/data"]

# Set the default shell to zsh
SHELL ["/bin/zsh", "-c"]

# install pnpm
RUN npm install -g pnpm

# Set the working directory
WORKDIR /app
