# Deploying Containers to Google Container Engine
[Google Container Engine](https://cloud.google.com/container-engine/) is powered
by [Kubernetes](https://kubernetes.io/), which manages containerized applications
and automates deployment and scaling.

## Install Google Cloud SDK and k8s Client
After installing [gcloud](https://cloud.google.com/sdk/docs) and `kubectl`,
(either via `gcloud` or [manually](https://kubernetes.io/docs/tasks/tools/install-kubectl/)),
initialize `gcloud`:

```
gcloud init
```

## Create a GCP Project
Use the [Google Console](https://console.cloud.google.com/projectcreate) to create
a project and configure `gcloud`:

```
gcloud config set project <PROJECT_ID>
```

Projects, that were already created, can also be selected as part of `gcloud init`.

## Push Docker Images to a Registry
When using [Google Container Registry](https://cloud.google.com/container-registry/)
Images must be tagged using the following format:

```
[HOSTNAME]/[PROJECT_NAME]/IMAGE_NAME[:TAG]
```

For example:

```
docker build -f ./api/Dockerfile -t gcr.io/k8s-micros6s/awesome-api:1.0.0 .
```

Then and push the image to remote:

```
gcloud docker -- push gcr.io/k8s-micros6s/awesome-api:1.0.0
```

Additional information about pulling and pushing images can be found [here](https://cloud.google.com/container-registry/docs/pushing-and-pulling).

## Create a Cluster
A Container Engine cluster is a group of Compute Engine instances running Kubernetes.
It consists of one or more node instances, and a managed Kubernetes master endpoint.

Creating a cluster can be done from the [Google Console](https://console.cloud.google.com/kubernetes/add) or by running the following command:

```
gcloud container clusters create <CLUSTER_NAME> --zone <ZONE> --machine-type <MACHINE_TYPE>
```

For example:

```
gcloud container clusters create cluster-1 --zone europe-west1-b --machine-type f1-micro
```

More information can be found [here](https://cloud.google.com/container-engine/docs/clusters/).



## Connect k8s Client to the Created Cluster
```
gcloud container clusters get-credentials cluster-1 --zone europe-west1-b
```

## Create k8s Objects
Create Deployments, Services, etc by using the following command:

```
kubectl create -f <CONFIG_PATH>
```

## External Services
It might take a bit for the external IP to become available, use this command while
waiting:

```
kubectl get services --watch
```

# Cleaning Up
All k8s Objects can be deleted by running:

```
kubectl delete all --all
```

The cluster can be deleted by running:

```
gcloud container clusters delete <CLUSTER_NAME> --zone <ZONE>
```
