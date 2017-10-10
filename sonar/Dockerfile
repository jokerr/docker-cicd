FROM openjdk:8

LABEL MAINTAINER="Josh Kerr" description="Sonar Docker instance"


# Environment variables
ENV SONAR_VERSION=6.5 \
    SONARQUBE_HOME=/opt/sonarqube \
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL=

# Http port
EXPOSE 9000

# Install Sonarqube
RUN set -ex \
    # pub   2048R/D26468DE 2015-05-25
    #       Key fingerprint = F118 2E81 C792 9289 21DB  CAB4 CFCA 4A29 D264 68DE
    # uid                  sonarsource_deployer (Sonarsource Deployer) <infra@sonarsource.com>
    # sub   2048R/06855C1D 2015-05-25
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE \
    && cd /opt \
    && curl -o sonarqube.zip -fSL https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    && curl -o sonarqube.zip.asc -fSL https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip.asc \
    && gpg --batch --verify sonarqube.zip.asc sonarqube.zip \
    && unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/*

# # Install plugins
# ENV PLUGIN_JAVA=4.14.0.11784 \
#     PLUGIN_GIT=1.2 \
#     PLUGIN_JS=3.2.0.5506 \
#     PLUGIN_PYTHON=1.8.0.1496

# RUN mkdir -p $SONARQUBE_HOME/extensions/plugins/
# ADD http://central.maven.org/maven2/org/sonarsource/java/sonar-java-plugin/${PLUGIN_JAVA}/sonar-java-plugin-${PLUGIN_JAVA}.jar $SONARQUBE_HOME/extensions/plugins/sonar-java-plugin-${PLUGIN_JAVA}.jar

VOLUME "${SONARQUBE_HOME}/data"

WORKDIR ${SONARQUBE_HOME}
COPY sonar.sh ${SONARQUBE_HOME}/bin/
ENTRYPOINT ["./bin/sonar.sh"]