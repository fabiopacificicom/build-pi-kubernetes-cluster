# Kubernetes cluster

[Overview](README.md) | [Prepare the pis](preparation.md) | [K0s](install_k0s) | [K3s](K3s_cluster.md) | [notes K101](notes_k8s_101.md) | [Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## PI Cluster setup | overview using K0s [ OK IT's WORKING ]

[Documentation](https://docs.k0sproject.io/v1.22.3+k0s.0/k0sctl-install/)

### Install K3sctl [local ubuntu machine]

Create a folder in your local machine and download the binaries

```bash
mkdir ~/k0s && cd ~/k0s

# Download the binary for linux x64
wget https://github.com/k0sproject/k0sctl/releases/download/v0.11.4/k0sctl-linux-x64

# Make the file executable
chmod u+x k0sctl-linux-x64

# Look the path directories
echo $PATH
# Copy the executable in a PATH's folder
cp k0sctl-linux-x64 /home/fab/.local/bin/k0sctl

# run k0sctl to see if it works
k0sctl

```

### Create a yaml config file

The command below will create a .yaml file in the current directory

```bash
k0sctl init > k0sctl.yaml

```

Edit the configuration file

```bash
apiVersion: k0sctl.k0sproject.io/v1beta1
kind: Cluster
metadata:
  name: k0s-cluster
spec:
  hosts:
  - ssh:
      address: 192.168.1.120 # master node ip
      user: ubuntu # machine username
      port: 22
      keyPath: /home/fab/.ssh/id_rsa # ssh key path
    role: controller
  - ssh:
      address: 192.168.1.121 # worker node ip
      user: ubuntu # machine username
      port: 22
      keyPath: /home/fab/.ssh/id_rsa # ssh key path
    role: worker
  k0s:
    version: 1.22.3+k0s.0

```

## Deploy the cluster

```bash
k0sctl apply --config k0sctl.yaml

```

## Access the cluster

generate a kubeconfig file and use it to access the cluster

```bash
# generates kubeconfig file
k0sctl kubeconfig > kubeconfig

# access the cluster via the kubeconfig file
kubectl get pods --kubeconfig kubeconfig -A # returns a list of all pods running in the deployed cluster

```

Or access using [lens](https://k8slens.dev/)

## Cheatsheet quick start docker book

### Continerize an app

### Build for multiple architectures

### Install Docker buildx (os: Ubuntu 20.04 | WSL 2 | ARM64)

[documentation](https://docs.docker.com/buildx/working-with-buildx/)

download binaries for my current Kernel architecture ARM64

```bash
## enter the .docker directory and create a cli-plugins folder 
cd ~/.docker && mkdir cli-plugins
## download buldx binaries in the current folder
curl https://github.com/docker/buildx/releases/download/v0.7.1/buildx-v0.7.1.linux-arm64
## rename the file and allow execution
mv buildx-v0.7.1.linux-arm64 docker-buildx
chmod a+x docker-buildx

## sets up docker builder command as an alias to docker buildx
docker buildx install
```

### Build and push the docker image to the registry

Build the docker image with buildx adding the --platform option and push it to the docker hub registry

```bash
Enter the project folder where the docker file is.
cd /Project/k8s/quick-start-book/App/
## build and push (build the image based on a Docker file, tag it and push it to the registry using my dockerhub id pacificdev
docker image build --push --platform linux/amd64,linux/arm64 -t pacificdev/qsk-book:1.0.11 .
## go back one directory
cd..
# apply pod.yaml file to deploy our application
kubectl apply -f pod.yml

```

the command `docker image build`, builds the image for multiple architectures ARM and AMD.

### Altrnatively push a local image to the docker hub registry

Read the docs and try to push a local image (need to push it for arm64 too)
[Docker docs](https://docs.docker.com/engine/reference/commandline/push/)
