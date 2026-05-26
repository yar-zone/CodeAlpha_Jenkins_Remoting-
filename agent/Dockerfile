FROM jenkins/inbound-agent:latest

USER root

# Install common build tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    make \
    && rm -rf /var/lib/apt/lists/*

USER jenkins

ENTRYPOINT ["/usr/local/bin/jenkins-agent"]
