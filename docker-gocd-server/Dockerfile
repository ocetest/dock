FROM centos:7
MAINTAINER Jerry Xiao <ocellus@163.com>

# Install more apk packages we might need
RUN set -x \
  && yum clean all \
  && yum -y install epel-release \
  && yum update -y \
  && yum install -y \
    git \
    httpd-tools \
    java-1.8.0-openjdk \
    subversion \
    xmlstarlet \
    unzip \
  && yum clean all

# Add go user and group
RUN groupadd --gid 500 go && adduser --shell /bin/bash --home /var/lib/go-server --no-create-home --uid 500 -g go go

# Install GoCD Server from zip file
ARG GO_MAJOR_VERSION=17.4.0
ARG GO_BUILD_VERSION=4892
ARG GO_VERSION="${GO_MAJOR_VERSION}-${GO_BUILD_VERSION}"
ARG GOCD_SHA256=18e7da698804d329b8b79b8d49f122e8dbc959d4e9019a0946c671559c872fcd

COPY go-server-17.4.0-4892.zip /tmp/go-server.zip
RUN set -x \
 
#  && curl -L --silent https://download.gocd.io/binaries/${GO_VERSION}/generic/go-server-${GO_VERSION}.zip \
#       -o /tmp/go-server.zip \
  && echo "${GOCD_SHA256}  /tmp/go-server.zip" | sha256sum -c - \
  && unzip /tmp/go-server.zip -d /usr/local \
  && ln -s /usr/local/go-server-${GO_MAJOR_VERSION} /usr/local/go-server \
  && chown -R go:go /usr/local/go-server-${GO_MAJOR_VERSION} \
  && rm /tmp/go-server.zip

RUN set -x && mkdir -p /etc/default \
  && cp /usr/local/go-server-${GO_MAJOR_VERSION}/go-server.default /etc/default/go-server \
  && sed -i -e "s/DAEMON=Y/DAEMON=N/" /etc/default/go-server

RUN set -x && mkdir /etc/go && chown go:go /etc/go && chmod -R 770 /etc/go \
  && mkdir /var/lib/go-server && chown go:go /var/lib/go-server && chmod -R 770 /var/lib/go-server \
  && mkdir /var/log/go-server && chown go:go /var/log/go-server && chmod -R 770 /var/log/go-server
RUN set -x && touch  /etc/go/log4j.properties && chown -R go:go /etc/go/log4j.properties && chmod -R 770 /etc/go/log4j.properties
# Expose ports needed
EXPOSE 8153 8154
EXPOSE 22 

VOLUME /etc/go
VOLUME /var/lib/go-server
VOLUME /var/log/go-server

# add the entrypoint config and run it when we start the container
COPY ./docker-entrypoint.sh /
RUN chown go:go /docker-entrypoint.sh && chmod 500 /docker-entrypoint.sh

# With the upgrade to java8 we need to add this option to prevent curl and
# other SSL options inside Go from breaking.
ENV GO_SERVER_SYSTEM_PROPERTIES="-Dcom.sun.net.ssl.enableECC=false"
#RUN     /bin/echo 'root:123456' |chpasswd
#RUN     /bin/echo 'go:123456' |chpasswd
#CMD    /usr/sbin/sshd -D

USER  root
ENTRYPOINT ["/docker-entrypoint.sh"]
