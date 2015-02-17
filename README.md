# haproxy-confd-demo

This repo illustrates how to load balance web containers in CoreOS using HAProxy and confd.

## Quickstart

### Clone this repo

```
$ git clone https://github.com/coopermaa/haproxy-confd-demo.git
$ cd haproxy-conf-demo
```

### Deploy the load balancer container

```
$ fleetctl start haproxy-confd.service
```

This will run HAProxy service at port 80 and port 1936 for statistics.

### Deploy web backend containers

Create two instances of nginx services and sidekick containers. These two nginx services will serve as our web backend:

```
$ fleetctl start nginx@1.service
$ fleetctl start nginx-discovery@1.service
$ fleetctl start nginx@2.service
$ fleetctl start nginx-discovery@2.service
```

### Test the application

Now, our web backend is load balanced. We can visit them via the load balancer.
First, find out where our haproxy-confd.service is deployed by running `fleetctl list-units`:

```
$ fleetctl list-units
UNIT                            MACHINE                         ACTIVE  SUB
haproxy-confd.service           beb76d18.../172.17.8.102        active  running
nginx-discovery@1.service       beb76d18.../172.17.8.102        active  running
nginx-discovery@2.service       f564b0fc.../172.17.8.101        active  running
nginx@1.service                 beb76d18.../172.17.8.102        active  running
nginx@2.service                 f564b0fc.../172.17.8.101        active  running
```

Take a note of the IP Address of the machine where haprox-confd is deployed. In my case, it's '172.17.8.102'. 
Now make some requests:

```
$ for i in {1..100}; do curl -s http://172.17.8.102 > /dev/null; done
```

and visit http://172.17.8.102:1936 to view the HAProxy statistics.