#
# MAINTAINER        Jerry Xiao <ocellus@163.com>
# DOCKER-VERSION    1.6.2
#
# Dockerizing Jenkins: Dockerfile for building Jenkins images
#
FROM       oce/jdk:1.7.0
MAINTAINER Jerry Xiao <ocellus@163.com>

ENV JENKINS_HOME /var/jenkins_home
ENV JENKINS_VERSION 2.46.1
ENV JENKINS_UC https://updates.jenkins-ci.org
ENV COPY_REFERENCE_FILE_LOG /var/log/copy_reference_file.log
RUN /bin/echo 'root:Chin@!2017' | chpasswd

RUN useradd -d "$JENKINS_HOME" -u 1000 -m -s /bin/bash jenkins

RUN /bin/echo 'jenkins:Chin@!201701' | chpasswd
RUN mkdir -p /usr/share/jenkins/ref/init.groovy.d

COPY init.groovy /usr/share/jenkins/ref/init.groovy.d/tcp-slave-agent-port.groovy

RUN curl -fL http://mirrors.jenkins-ci.org/war-stable/$JENKINS_VERSION/jenkins.war -o /usr/share/jenkins/jenkins.war && \
    chown -R jenkins "$JENKINS_HOME" /usr/share/jenkins/ref && \
    touch $COPY_REFERENCE_FILE_LOG && chown jenkins.jenkins $COPY_REFERENCE_FILE_LOG

ARG DOCKER_GID=999
USER jenkins:${DOCKER_GID}

VOLUME /var/jenkins_home

EXPOSE 8080 50000

COPY jenkins.sh /usr/local/bin/jenkins.sh

ENTRYPOINT ["/usr/local/bin/jenkins.sh"]
