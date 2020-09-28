# Deployment code for Annif-utils

Work in progress.

Helm chart to deploy Annif API to a Kubernetes/OKD cluster. Annif project data is populated from a tarball by
an initContainer when the pod starts. For general user guide, refer [documentation](https://github.com/NatLibFi/Annif/wiki/Getting-started)

If you are using external system for training new models, you should manually restart api-pod to force application to load new model data. 

## Setup the environment
### Docker/Kubernetes
Kubernetes on Docker is for testing purposes, for local use we recommend to use original Docker container from [NatLibFi Github](https://github.com/NatLibFi/Annif/wiki/Usage-with-Docker)

For local Docker/Kubernetes use you need kubectl (or oc) and ingress controller installed.

Here is how to deploy nginx ingress controller for Docker for Mac
https://kubernetes.github.io/ingress-nginx/deploy/

### OpenShift
oc should be installed 

### Both Kubernetes and Openshift:

For installation to Kubernetes and Openshift you need Helm installation:
[Install Helm](https://helm.sh/docs/intro/install/)


## Configuration

See *helm_charts/annif-utils/values.yaml*

## Deploy to local Kubernetes

Create a namespace and set the proper context. With local Kubernetes for Mac, with context set to local cluster: 
```bash
oc create namespace annif-utils
oc config set-context annif-utils-local --cluster=docker-desktop --user=docker-desktop --namespace=annif-utils
oc config use-context annif-utils-local
```

The defaults values should work for deploying:

```bash
helm install annif-utils helm_charts/annif-utils 
```

Note: The default ingress configuration assumes that all traffic coming to localhost is targeted for this application. 

## Deploy to an OKD cluster

Create a namespace and set the proper context. 
```bash
oc login # use whatever you usually do to start a session to OKD
oc new-project annif-utils 
```

Create a copy of a 'values.yaml' file for custom values, set at least ingressType and ingressHost. Choose a unique name for your ingress (maybe
match the namespace you created earlier). Here is example `example-custom-values.yaml`

```yaml
ingressType: Route
ingressHost: example-annif-utils.rahtiapp.fi
securityContext: {}
```

Deploy with Helm, using our custom values:

```bash
helm install annif-utils helm_charts/annif-utils -f example-custom-values.yaml 
```

## Change deployment parameters

You can change single parameter with command below. Please note that by using command with just --set option would use 
default values from values.yaml file instead of your custom values-file for the other parameters. That's why you should 
include also the `-f example-custom-values.yaml` part.


```bash
helm upgrade annif-utils helm_charts/annif-utils --set projectsTarballUrl=https://<<your_custom_url>> -f example-custom-values.yaml
```

## Funding:

<img src="./EU_logo.png" width="30%">
