# Kubernetes cluster

[Overview](README.md) | [Preparation](preparation.md) | [MasterNode](master_node.md) | [WorkerNode](worker_node.md)

## Install K3S (Master node)

Find your ip and run the installation script

### find your ip

```bash
ip a
```

### Installation script

run the installation script

```bash
curl -sfL https://get.k3s.io | sh -s - --bind-address 192.168.1.120
```

NOTE: Replace the ip address with your ip address

### Check the cluster info

run the command to check if the cluster is running

```bash
sudo kubectl cluster-info
```

NOTE: Wait for the output (be patient), you should see the folloing

```bash
Kubernetes control plane is running at https://192.168.1.120:6443
CoreDNS is running at https://192.168.1.120:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://192.168.1.120:6443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

If so far so good grab the token to connect a worker node to the master server node.

### find the token

You will need this to setup the worker node in the next step.

```bash
sudo cat /var/lib/rancher/k3s/server/node-token
//K109f2d555131da5174d4f0c1811669a44d1bb80165628523979a1e991554bdb8ac::server:91f513abc6e4b5e1e5c9ba23605c10ca
```

## Setup Worker Nodes

Follow the steps to setup all worker nodes [here](worker_node.md)

## List all nodes

Once all nodes have been configured following the steps in the point above, check if 
they are ready.

```bash
sudo kubectl get nodes
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

## Install Kubernetes Dashboard (Master node)

To manage Kubernetes you can also install a UI. Follow these steps [here](install_dashboard.md)

## Next

Once everything is set up check the fist steps page [here](first_steps.md.md)