# Deployment code for Annif-utils

Work in progress.

Currently has deploys Annif API with Helm to a Kubernetes/OKD cluster. Annif project data is populated from a tarball by
an initContainer when the pod starts.

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

Create a file for custom values, set at least ingressType and ingressHost. Choose a unique name for your ingress (maybe
match the namespace you created earlier). Here is example `example-annif-utils-rahti.yaml`

```yaml
ingressType: Route
ingressHost: example-annif-utils.rahtiapp.fi
```

Deploy with Helm, using our custom values:

```bash
helm install annif-utils helm_charts/annif-utils -f example-annif-utils-rahti.yaml 
```

## Change deployment parameters

You can change single parameter with command below. Please note that by using command with just --set option would use 
default values from values.yaml file instead of your custom values-file for the other parameters. That's why you should 
include also the `-f example-annif-utils-rahti.yaml` part.


```bash
helm upgrade annif-utils helm_charts/annif-utils --set projectsTarballUrl=https://<<your_custom_url>> -f example-annif-utils-rahti.yaml
```
