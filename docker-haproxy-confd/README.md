# docker-haproxy-confd

Dockerfile for haproxy and confd. This repo illustrates how to load balance
 web containers in CoreOS using HAProxy and confd.

## To Build image

```
docker build -t <user name>/haproxy-confd .
```

## Usage

You must pass the etcd peer address with the -e flag in docker run so that confd can connect to your etcd cluster:

```
docker run --name %n -p ${COREOS_PUBLIC_IPV4}:80:80 \
  -p ${COREOS_PRIVATE_IPV4}:1936:1936 -e "ETCD=http://${COREOS_PRIVATE_IPV4}:4001" coopermaa/haproxy-confd:latest
```

This will run HAProxy service at port 80 and port 1936 for statistics.

## Demo

See [haproxy-confd-demo](https://github.com/coopermaa/haproxy-confd-demo) for a demonstration.