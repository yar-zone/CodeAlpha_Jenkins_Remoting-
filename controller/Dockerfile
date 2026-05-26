FROM jenkins/jenkins:lts

# Skip the initial setup wizard
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

# Install recommended plugins
RUN jenkins-plugin-cli --plugins \
    git \
    workflow-aggregator \
    ssh-slaves \
    matrix-auth \
    credentials-binding \
    configuration-as-code

# Copy the Jenkins Configuration as Code file
COPY jenkins.yaml /var/jenkins_home/jenkins.yaml
ENV CASC_JENKINS_CONFIG=/var/jenkins_home/jenkins.yaml

EXPOSE 8080 50000
