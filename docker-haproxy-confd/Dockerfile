FROM ubuntu:14.04
MAINTAINER MAINTAINER coopermaa77@gmail.com

# Suppress dialogs for information during package installing
# and/or updating packages and use the default instead
ENV DEBIAN_FRONTEND noninteractive

# Let's install haproxy-1.5 (which has native ssl supported)
# add-apt-repository instruction is part of software-properties-common
# (formerly python-software-properties)
RUN apt-get update -q && \
    apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:vbernat/haproxy-1.5 && \
    apt-get install -y haproxy curl && \
    sed -i 's/^ENABLED=.*/ENABLED=1/' /etc/default/haproxy 

# confd and haproxy template
WORKDIR /usr/local/bin
RUN cd /usr/local/bin && \
    curl -fsSL https://github.com/kelseyhightower/confd/releases/download/v0.7.1/confd-0.7.1-linux-amd64 -o confd && \
    chmod a+x confd && \
    mkdir -p /etc/confd/{conf.d,templates}

ADD /haproxy.toml /etc/confd/conf.d/haproxy.toml
ADD /haproxy.tmpl /etc/confd/templates/haproxy.tmpl

# add confd-watch script
ADD /confd-watch /usr/local/bin/confd-watch
RUN chmod a+x /usr/local/bin/confd-watch

EXPOSE 80
CMD ["/usr/local/bin/confd-watch"]
