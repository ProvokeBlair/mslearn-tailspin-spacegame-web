FROM mcr.microsoft.com/devcontainers/dotnet:6.0

# Install NodeJS
ARG NODE_VERSION="16"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION}"; fi

# Install Gulp CLI globally
RUN npm install --global gulp-cli

# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# Optional: Install zsh
ARG INSTALL_ZSH="true"
ARG UPGRADE_PACKAGES="false"

# Set up non-root user and install common dependencies
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
COPY library-scripts/*.sh /tmp/library-scripts/
RUN bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true"

# Set working directory for Azure DevOps agent
WORKDIR /home/vscode/azure-pipelines

# Define Azure DevOps agent version
ARG ARCH="x64"
ARG AGENT_VERSION="4.251.0"

# Download and extract the agent
RUN curl -O -L https://vstsagentpackage.azureedge.net/agent/${AGENT_VERSION}/vsts-agent-linux-${ARCH}-${AGENT_VERSION}.tar.gz \
    && tar xzf vsts-agent-linux-${ARCH}-${AGENT_VERSION}.tar.gz \
    && chmod +x ./bin/installdependencies.sh \
    && ./bin/installdependencies.sh

# Copy start script and ensure permissions
COPY library-scripts/start.sh /home/vscode/azure-pipelines/start.sh
RUN chown vscode:vscode /home/vscode/azure-pipelines/start.sh \
    && chmod +x /home/vscode/azure-pipelines/start.sh

# Clean up
RUN rm -rf /var/lib/apt/lists/* /tmp/library-scripts /home/vscode/azure-pipelines/*.tar.gz

# Default working directory
WORKDIR /home/vscode
