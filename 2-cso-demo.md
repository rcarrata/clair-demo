# Container Security Operator Demo

Using the Container Security Operator, (CSO) you can scan container images associated with active
pods, running on OpenShift (4.2 or later) and other Kubernetes platforms, for known vulnerabilities.

1. Install the CSO operators

Operators -> Container Security -> Subscribe (all namespaces + automatic)

The Quay Container Security operator will be installed. Check the status in the Installed Operators

![CVO Install](/pics/cso0.jpg)

[![](/pics/cso0.png "CVO Install")]({{site.url}}/pics/cso0.png)

2. Create a deployment based in one image that is already in Quay.io

```
oc create deployment recommendation --image=quay.io/rcarrata/recommendation:vertx
```

```
oc get pod -w

```

3. Check the Console Overview and into the Image Vulnerabilities check the vulnerabilities from for
   pods, running the different images through different


```
oc get imagemanifestvulns -n test
NAME                                                                      AGE
sha256.215bcb1c259e3c4555356f4233a4208dd74545a2a60e95a3e599c60239d83c4b   2m
```
