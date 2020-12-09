# Quay and Clair PoC Demo

## Install CLAIR and QUAY in Openshift

1. Create the project

```
oc new-project quay-enterprise

oc project quay-enterprise
```

2. Install the Operator through Operator Hub (TODO: automate the installation of the operator)

<img src="pics/quay0.png" alt="quay0" width="500"/>

3. Create the secret (https://access.redhat.com/solutions/3533201)

```
oc create -f templates/redhat-pull-secret.yaml
```

3. Create a Quay Instance and Deploy Clair

```
oc apply -f templates/quay-instance.yaml
```

4. Push one sample image with some vulnerabilities to the new QUAY

Quay User default:

```
user: quay
password: password
```

