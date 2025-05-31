# ImagePullBackoff

When a kubelet starts creating containers for a Pod using a container runtime, it might be possible the container is in Waiting state because of ImagePullBackOff.

The status ImagePullBackOff means that a container could not start because Kubernetes could not pull a container image for reasons such as 

- Invalid image name or 
- Pulling from a private registry without imagePullSecret. 

The BackOff part indicates that Kubernetes will keep trying to pull the image, with an increasing back-off delay.

Kubernetes raises the delay between each attempt until it reaches a compiled-in limit, which is 300 seconds (5 minutes).

🔑 What is imagePullSecret?
It is a Kubernetes Secret of type kubernetes.io/dockerconfigjson (or the older dockercfg type), which contains credentials for accessing a private image registry such as:

Red Hat Quay

Docker Hub

JFrog Artifactory

AWS ECR, Azure ACR, GCR, etc.

📦 Use Case
When you want OpenShift to deploy an image from a private registry, you need to tell it how to authenticate with that registry. That’s where imagePullSecret is used.

🔧 How to Create One
```
oc create secret docker-registry my-pull-secret \
  --docker-server=REGISTRY_URL \
  --docker-username=USERNAME \
  --docker-password=PASSWORD \
  --docker-email=EMAIL
```
  
🔗 How to Use It

1. Link it to a ServiceAccount
   
To make all pods use it:

```
oc secrets link default my-pull-secret --for=pull

```
**default** is the default service account used by most pods unless another is specified.

2. Specify in a Pod or Deployment YAML
   
```
apiVersion: v1
kind: Pod
metadata:
  name: private-image-pod
spec:
  containers:
  - name: app
    image: myprivateregistry.com/myapp:latest
  imagePullSecrets:
  - name: my-pull-secret

```
