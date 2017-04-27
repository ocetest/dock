#
# MAINTAINER        Jerry Xiao <ocellus@163.com>
# DOCKER-VERSION    1.6.2
#
# Dockerizing jre: Dockerfile for building jre images
#
FROM       oce/centos:7.1
MAINTAINER Jerry Xiao <ocellus@163.com>

ENV JAVA_VERSION 1.7.0
ENV JRE_HOME /usr/lib/jvm/jre

# Install jre
RUN yum -y install java-${JAVA_VERSION}-openjdk mysql && \
    yum clean all
