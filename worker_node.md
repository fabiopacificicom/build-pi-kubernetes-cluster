# Kubernetes cluster

[Overview](README.md) | [Preparation](preparation.md) | [MasterNode](master_node.md) | [WorkerNode](worker_node.md)

## Intall K3S on a worker node

Now you need to follow these steps on all PIs that you want to use as worker nodes. 

### Access the node via ssh

Login into the node via ssh and run the installation script

```bash
ssh ubuntu@192.168.1.121
```

### run the installation script

Run the installation script on the worker node, pass the K3S_URL with the master node ip address and K3S_TOKEN with the actual token taken from the master node in the previous steps.

Sintax:  `curl -sfL https://get.k3s.io | K3S_URL=master_node_ip_address_and_port_here K3S_TOKEN=my_node_token_here sh -`

```bash
curl -sfL https://get.k3s.io | K3S_URL=https://192.168.1.120:6443 K3S_TOKEN=K109f2d555131da5174d4f0c1811669a44d1bb80165628523979a1e991554bdb8ac::server:91f513abc6e4b5e1e5c9ba23605c10ca sh -

```

Repeat these steps on all Worker nodes to want to add to the cluster.

## Next

Now let's move the first steps in a kubernetes cluster [here](first_steps.md.md)
