# Testing Cernan on a K8s cluster

This is a set of kubernetes manifests as well as some supporting
documentation for getting heapster up and running on K8s. The project
doesn't currently work, heapster doesn't seem to be sending the statsd
data correctly to the Cernan instance.

# Setup

For some reason the vanilla Cernan docker image has been incorrectly
configured, so I have written a simple extension layer which redefines
the entrypoint/cmd config options.

``` shell
$ docker build -f Dockerfile.cernan --name moredhel/cernan:v3 .
```

The above dockerfile bakes a cernan.toml into the image for easier transport.


I tested this on the GKE platform, so needed to setup cluster-wide
permissions for my account (Google has decent documentation on how to
do this).

Then you need to run a second daemon-set of Heapster instances with
custom flags to export to a custom statsd aggregator (where we will be
running Cernan).

``` shell
$ kubectl apply -f heapster.yaml
```


We finally we deploy our Cernan instance (currently outputing
telemetry to stdout).

``` shell
$ kubectl apply -f cernan.yaml
```

I didn't manage to get this working as I ran out of time, but all
components are working, service discovery seems to be the final hurdle
before everything is working correctly.
