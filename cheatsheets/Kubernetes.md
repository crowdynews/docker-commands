# Kubernetes Commands

Cheatsheet of usefull `kubectl` commands to run on a Kubernetes cluster.

**Kubernetes `v1.7` or greater.**

* [The kubectl overview](https://kubernetes.io/docs/user-guide/kubectl-overview/)
* [Complete command list of v1.7](https://kubernetes.io/docs/user-guide/kubectl/v1.7/)

## Create Resource

Create a resource or a cluster from a file or a directory.

```
kubectl create -f <FILENAME|DIRECTORY>
```

## Inspect Resources

Display information of all resources or of a specific resource:

```
kubectl get <RESOURCE>
```

* `kubectl get pods` or `kubectl get po`
* `kubectl get deployments` or `kubectl get deploy`
* `kubectl get relplicasets` or `kubectl get rs`
* `kubectl get services` or `kubectl get svc`
* `kubectl get pods` or `kubectl get no`

With the `-o,--output` you can format the resource result, e.g.:

* `kubectl get all -o name` to get all the resources names
* `kubectl get all -o json` to get all the resources as json
* `kubectl get all -o yaml` to get all the resources as yaml

To export the json or yaml output, of your cluster, to a file execute respectively:

* `kubectl get all -o json > <FILENAME>.json` or
* `kubectl get all -o yaml > <FILENAME>.yml`

## Remove Resources

Remove a resource:

```
kubectl delete <resource> <name>
```

e.g.
`kubectl delete pods redis-1891543903-w1699` or `kubectl delete po/redis-1891543903-w1699`

Remove all resources of a cluster:

```
kubectl delete all --all
```

## Configure a Private Image Registry

Pull images from a private docker image registry:

```
kubectl create secret docker-registry <REGISTRY_NAME> --docker-server=<DOCKER_REGISTRY_URL> --docker-username=<USERNAME> --docker-password=<PASSWORD> --docker-email=<EMAIL>
```

e.g.

```
kubectl create secret docker-registry myPrivateRegistry --docker-server=https://myprivateregisrty.com --docker-username=superUser --docker-password=password123 --docker-email=user@privatedocker.com
```

## Context and Configuration

The _Context_ defines with which kubernetes cluster the `kubectl` client communicates with.

To view the entire (merged) _kubeconfig_:

```
kubectl config view
```

To get all contexts:

```
kubectl config get-contexts
```

Switch to a different context:

```
kubectl config use-context <CONTEXT_NAME>
```

## Execute commands

You can execute commands in a container in a pod with the following command:

```
kubectl exec -it <POD_NAME> <COMMAND>
```

e.g. in order to run the redis-cli

```
kubectl exec -it redis redis-cli
```

## Copy files and directories

From Pod to local host:

```
kubectl cp <POD>:<RESOURCE> <LOCAL_DESTINATION>
```

for example:

```
kubectl cp mongo-0:/tmp/dump ./tmp/dump
```

From local host to Pod:

```
kubectl cp <LOCAL_DESTINATION> <POD>:<RESOURCE>
```

for example:

```
kubectl cp ./config.json mongo-0:/config
```

## Resources

* [kubectl cheatsheets](https://kubernetes.io/docs/user-guide/kubectl-cheatsheet/)
