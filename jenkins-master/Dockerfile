FROM jenkins

LABEL MAINTAINER="Josh Kerr" description="Jenkins Docker instance"

USER root
RUN mkdir /var/log/jenkins
RUN mkdir /var/cache/jenkins
RUN chown -R jenkins:jenkins /var/log/jenkins
RUN chown -R jenkins:jenkins /var/cache/jenkins

# Install plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# Skip initial setup
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false

USER jenkins