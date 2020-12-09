## Install CLAIR and QUAY in Openshift


1. Create the project

```
oc new-project quay-enterprise

oc project quay-enterprise
```

2. Install the Operator through Operator Hub (TODO: automate the installation of the operator)

3. Create the secret

```
apiVersion: v1
kind: Secret
metadata:
  name: redhat-pull-secret
data:
  .dockerconfigjson: ewogICJhdXRocyI6IHsKICAgICJxdWF5LmlvIjogewogICAgICAiYXV0aCI6ICJjbVZrYUdGMEszRjFZWGs2VHpneFYxTklVbE5LVWpFMFZVRmFRa3MxTkVkUlNFcFRNRkF4VmpSRFRGZEJTbFl4V0RKRE5GTkVOMHRQTlRsRFVUbE9NMUpGTVRJMk1USllWVEZJVWc9PSIsCiAgICAgICJlbWFpbCI6ICIiCiAgICB9CiAgfQp9
type: kubernetes.io/dockerconfigjson
```

```
oc create -f redhat-pull-secret.yaml
```

3. Create a Quay Instance and Deploy Clair

```
apiVersion: redhatcop.redhat.io/v1alpha1
kind: QuayEcosystem
metadata:
  name: rober-quayecosystem
spec:
  quay:
    imagePullSecretName: redhat-pull-secret
  clair:
    enabled: true
    imagePullSecretName: redhat-pull-secret
```

```
oc apply -f quay-instance.yaml
```

4. Push one sample image with some vulnerabilities to the new QUAY

* User default: user=quay/password=password

```
podman pull quay.io/rcarrata/customer

export QUAY_URL=$(oc get route -n quay-enterprise rober-quayecosystem-quay -o jsonpath={.spec.host})
ID=$(podman images quay.io/rcarrata/customer | grep rcarrata | awk '{ print $3 }')

podman --tls-verify=false login $QUAY_URL

podman tag $ID $QUAY_URL/quay/customer

podman push --tls-verify=false $QUAY_URL/quay/customer
```

5. Check in the QUAY_URL the images and check the vulnerabilities

```
echo $QUAY_URL
```

#### This PoC is successfully tested in Quay v3.3 and Clair v3.3.1

