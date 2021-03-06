FROM jenkins:2.60.3

LABEL MAINTAINER="Josh Kerr" description="Jenkins Docker instance"

USER root
# getting the docker-cli
# --- Attention: docker.sock needs to be mounted as volume in docker-compose.yml
# see: https://issues.jenkins-ci.org/browse/JENKINS-35025
# see: https://get.docker.com/builds/
# see: https://wiki.jenkins-ci.org/display/JENKINS/CloudBees+Docker+Custom+Build+Environment+Plugin#CloudBeesDockerCustomBuildEnvironmentPlugin-DockerinDocker
RUN curl -sSL -O https://get.docker.com/builds/Linux/x86_64/docker-latest.tgz && tar -xvzf docker-latest.tgz
RUN mv docker/* /usr/bin/


USER jenkins

# Install plugins - see https://github.com/jenkinsci/docker#preinstalling-plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# setup Gradle and Maven
COPY config/hudson.plugins.gradle.Gradle.xml /var/jenkins_home/hudson.plugins.gradle.Gradle.xml
COPY config/hudson.tasks.Maven.xml /var/jenkins_home/hudson.tasks.Maven.xml
COPY config/org.jenkinsci.plugins.configfiles.GlobalConfigFiles.xml /var/jenkins_home/org.jenkinsci.plugins.configfiles.GlobalConfigFiles.xml

# setup initial job
COPY jobs /var/jenkins_home/jobs

# skip plugin prompts
RUN echo 2.0 > /usr/share/jenkins/ref/jenkins.install.UpgradeWizard.state

# Skip initial setup
# ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false