# Kubernetes Workshop on CLT 2019

Fork of [ondrejsika/kubernetes-training-example](https://github.com/ondrejsika/kubernetes-training-examples)

---

## Install

### Kubectl

<https://kubernetes.io/docs/tasks/tools/install-kubectl/>

#### Linux

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl
```

### Minikube

<https://kubernetes.io/docs/tasks/tools/install-minikube/#install-minikube>

#### Linux

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.30.0/minikube-linux-amd64 && chmod +x minikube && sudo cp minikube /usr/local/bin/ && rm minikube
```

### Bash Completion

```
source <(kubectl completion bash)
source <(minikube completion bash)
```

Or save to `.bashrc`

```
echo "source <(kubectl completion bash)" >> ~/.bashrc
echo "source <(minikube completion bash)" >> ~/.bashrc
```


### Start Minikube

```
minikube start --kubernetes-version v1.13.2
```

### Dashboard

```
minikube dashboard
```

or

```
kubectl proxy
```

And go to <http://127.0.0.1:8001/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy>

## Examples

### Get Nodes

```
kubectl get nodes
```

### Proxy to cluster

Start proxy

```
kubectl proxy
```

### Create Pod

```
kubectl apply -f 01_pod.yml
kubectl apply -f 02_pod.yml
```

### List Pods

```
kubectl get pods
kubectl get po
```

See:

- http://127.0.0.1:8001/api/v1/namespaces/default/pods/simple-hello-world/proxy/
- http://127.0.0.1:8001/api/v1/namespaces/default/pods/multi-container-pod/proxy/


### Exec (Connect) Pod

```
kubectl exec -ti multi-container-pod bash
```

### Delete Pod

```
kubectl delete -f 01_pod.yml
kubectl delete -f 02_pod.yml
```

### Create Replica Set

```
kubectl apply -f 03_01_replica_set.yml
```

### List Replica Sets

```
kubectl get replicasets
kubectl get rs
kubectl get rs,po
```

### Update Replica Set

See the difference

```
vimdiff 03_01_replica_set.yml 03_02_replica_set.yml
```

```
kubectl apply -f 03_02_replica_set.yml
```

### Delete Replica Set

```
kubectl delete -f 03_01_replica_set.yml
```

### Create Deployment

```
kubectl apply -f 04_01_deployment.yml
```

### List Deployments

```
kubectl get deployments
kubectl get deploy
kubectl get po,rs,deploy
```

### Expose Deployment Set (Create Service)

```
kubectl expose deploy hello-world --type=NodePort
```

See: http://127.0.0.1:8001/api/v1/namespaces/default/services/hello-world/proxy/

Or using `minikube service` (just for NodePort)

```
minikube service hello-world
```


### Update Deployment

See the difference

```
vimdiff 04_01_deployment.yml 04_02_deployment.yml
```

```
kubectl apply -f 04_02_deployment.yml
```

See: http://127.0.0.1:8001/api/v1/namespaces/default/services/hello-world/proxy/

### Delete Deployment

```
kubectl delete -f 04_01_deployment.yml
# and service
kubectl delete svc/hello-world
```

### Create Service ClusterIP

Create deploymnet again:

```
kubectl apply -f 04_02_deployment.yml
```

And create service:

```
kubectl apply -f 05_clusterip_service.yml
```

See: http://127.0.0.1:8001/api/v1/namespaces/default/services/hello-world-clusterip/proxy/

### Create Service NodePort

```
kubectl apply -f 06_nodeport_service.yml
```

See: http://127.0.0.1:8001/api/v1/namespaces/default/services/hello-world-nodeport/proxy/

### List Services

```
kubectl get services
kubectl get svc
kubectl get po,rs,deploy,svc
kubectl get all
```

### Delete Service

```
kubectl apply -f 05_clusterip_service.yml
kubectl apply -f 06_nodeport_service.yml
# and deployment
kubectl delete -f 04_01_deployment.yml
```

### Deploy Application (Multiple Deployments and Services)

```
kubectl apply -f 07_counter.yml
```

See: http://127.0.0.1:8001/api/v1/namespaces/default/services/counter/proxy/

### List components

```
kubectl get all -l project=counter
```

### Open in Browser

```
minikube service counter
```

### Delete Application

```
kubectl delete deploy,svc -l project=counter
```

### Create Namespace

```
kubectl create namespace counter
```

or

```
kubectl apply -f 08_namespace.yml
```

### List Namespaces

```
kubectl get namespaces
kubectl get ns
```

### Deploy to Namespace

```
kubectl apply -f 07_counter.yml -n counter
```

See: http://127.0.0.1:8001/api/v1/namespaces/counter/services/counter/proxy/

### Delete Namespace

```
kubectl delete ns/counter
```

## Ingress

### Enable Ingress on Minikube

```
minikube addons enable ingress
```

### Create Ingress

Create some services (& deploymnets)

```
kubectl apply -f webservers.yml
```

See:

- http://127.0.0.1:8001/api/v1/namespaces/default/services/nginx/proxy/
- http://127.0.0.1:8001/api/v1/namespaces/default/services/apache/proxy/


Create Ingress

```
kubectl apply -f 10_ingress.yml
```

See:

- http://nginx.192.168.99.100.xip.io
- http://apache.192.168.99.100.xip.io


### Delete it

```
kubectl delete -f webservers.yml
kubectl delete -f 10_ingress.yml
```

---

Do you like this workshop?

Tweet about it with @ondrejsika and #clt2019

Thanks!

---

Do you want more? Try my

## Kubernetes Training in Dresden

- Theory introduction to Kubernetes
- How to install Minikube and kubectl
- Description of Kubernetes components
- Deployment to Kubernetes
- Working with permissions in the Kubernetes cluster
- Theory introduction to Helm packages
- Installation/Deployment using Helm
- Creating a custom Helm package

I also offer training for:

- Git
- Gitlab CI
- Docker
- Ansible

For further informations, write me email to <ondrej@sika-kraml.de>.

Training are also possibe in German. For German courses wite to <schulungen@sika-kraml.de>