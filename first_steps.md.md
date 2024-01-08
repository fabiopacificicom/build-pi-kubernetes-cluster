# Kubernetes cluster

[Overview](README.md) | [Preparation](preparation.md) | [MasterNode](master_node.md) | [WorkerNode](worker_node.md)

## List all nodes

Show basic node details

```bash
ubuntu@ubuntu:~$ sudo kubectl get nodes
NAME       STATUS   ROLES                  AGE     VERSION
ubuntun1   Ready    <none>                 4m22s   v1.21.5+k3s2
ubuntu     Ready    control-plane,master   31m     v1.21.5+k3s2
```

Show a mode details about the nodes

```bash
ubuntu@ubuntu:~$ sudo kubectl get nodes -o wide
NAME       STATUS   ROLES                  AGE    VERSION        INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
ubuntu     Ready    control-plane,master   30m    v1.21.5+k3s2   192.168.1.120   <none>        Ubuntu 20.04.3 LTS   5.4.0-1044-raspi   containerd://1.4.11-k3s1
ubuntun1   Ready    <none>                 3m9s   v1.21.5+k3s2   192.168.1.121   <none>        Ubuntu 20.04.3 LTS   5.4.0-1044-raspi   containerd://1.4.11-k3s1
```

## Learn Kubernetes Basics

hello minikube [https://kubernetes.io/docs/tutorials/hello-minikube/]
Pods[https://kubernetes.io/docs/concepts/workloads/pods/]
deployments [https://kubernetes.io/docs/concepts/workloads/controllers/deployment/]
k basics [https://kubernetes.io/docs/tutorials/kubernetes-basics/]
  - https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/


## NEXT STEPS

- create a deployment
deployments [https://kubernetes.io/docs/concepts/workloads/controllers/deployment/]

### Create a deployment of an app using a docker image from docker hub registry

The image is from nginx and -r to create two replica

```bash
sudo kubectl create deployment kubernets-test-one --image=nginx -r 2
```

### Get the deployments

```bash
sudo kubectl get deployments
```

### View the app

#### Stat a proxy

Open a new terminal, ssh into the master node and start a proxy to enable network communications

Window no.2

```bash
sudo kubectl proxy
```

### verify you have access, from the other terminal

window no.1.

```bash
curl http://localhost:8001/version
```

Get the por name and save it inside an environment variable

window no.1 (master node)

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME

curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/
```

### View the log

```bash
kubectl logs $POD_NAME

```


### Start a bash session

```bash
kubectl exec -ti $POD_NAME -- bash
```

(Continue reading here) [https://kubernetes.io/docs/tutorials/kubernetes-basics/)


## FROM DOCKER COMPOSE TO KUBERNETES
https://kubernetes.io/docs/tasks/configure-pod-container/translate-compose-kubernetes/


## Add a self signed certificate openssl to the master node (DON'N NEED THIS)
https://kubernetes.io/docs/tasks/administer-cluster/certificates/
