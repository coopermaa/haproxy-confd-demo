FROM debian:wheezy
MAINTAINER MAINTAINER coopermaa77@gmail.com

# Suppress dialogs for information during package installing
# and/or updating packages and use the default instead
ENV DEBIAN_FRONTEND noninteractive

# Let's install haproxy-1.5 (which has native ssl supported)
# For Debian/Ubuntu HAProxy packages, see: http://haproxy.debian.net/
# We ignore the Valid-Until header within Release files, in order to prevent ("Release file expired") error
RUN echo "deb http://cdn.debian.net/debian wheezy-backports main" >> /etc/apt/sources.list.d/backports.list && \
    apt-get -o Acquire::Check-Valid-Until=false update && \
    apt-get install -y haproxy -t wheezy-backports && \
    apt-get install -y curl && \
    sed -i 's/^ENABLED=.*/ENABLED=1/' /etc/default/haproxy 

# confd and haproxy template
WORKDIR /usr/local/bin
RUN cd /usr/local/bin && \
    curl -fsSL https://github.com/kelseyhightower/confd/releases/download/v0.7.1/confd-0.7.1-linux-amd64 -o confd && \
    chmod a+x confd 

ADD etc/confd/ /etc/confd

EXPOSE 80 1936
# Note: we use shell form here, exec form will not do variable substitution on $ETCD
CMD /usr/local/bin/confd -debug -verbose -interval 60 -node $ETCD
