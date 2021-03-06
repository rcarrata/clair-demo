# Container Security Operator Demo

Using the Container Security Operator, (CSO) you can scan container images associated with active
pods, running on OpenShift (4.2 or later) and other Kubernetes platforms, for known vulnerabilities.

### Install Container Security Operator

1. Install the CSO operators

```
Operators -> Container Security -> Subscribe (all namespaces + automatic)
```

The Quay Container Security operator will be installed. Check the status in the Installed Operators

<img src="pics/cso0.png" alt="cso0" width="800"/>

### Deploy example application from Quay registry

2. Create a deployment based in one image that is already in Quay.io

```
oc new-project test && oc project test
oc create deployment recommendation --image=quay.io/rcarrata/recommendation:vertx
```

```
oc get pod
```

### Check the vulnerabilities

3. Check the Console Overview and into the Image Vulnerabilities check the vulnerabilities from for
   pods, running the different images through different

<img src="pics/cso1.png" alt="cso1" width="250"/>

4. Check the Vulnerabilities of the pods using the images what we checked.

* Go to Administration -> Image Vulnerabilities and select the namespace (our case is "test")

<img src="pics/cso2.png" alt="cso2" width="600"/>

5. You can open the Manifest also to check the vulnerability manifest in the source registry (Quay)

<img src="pics/cso4.png" alt="cso4" width="600"/>

6. Also the vulnerabilities can be check by CLI, with the CRD imagemanifestvulns (or vulns)

```
oc get crds | grep vuln
imagemanifestvulns.secscan.quay.redhat.com                  2020-12-09T16:34:53Z
```

In the namespace of test, get the image manifest vulns

```
oc get imagemanifestvulns -n test
NAME                                                                      AGE
sha256.215bcb1c259e3c4555356f4233a4208dd74545a2a60e95a3e599c60239d83c4b   2m
```

7. Inside of the Image Manifest Vulns we can check the details about the vulnerabilities

<img src="pics/cso3.png" alt="cso2" width="500"/>

Notice that is the exact same information as the source, sorted and structure and accesible from
Openshift Console.

