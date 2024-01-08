# Kubernetes cluster

[Overview](README.md) | [Preparation](preparation.md) | [MasterNode](master_node.md) | [WorkerNode](worker_node.md)

## Install Dashboard | Master Node

Documentation [here](https://rancher.com/docs/k3s/latest/en/installation/kube-dashboard/)

Run the following command on the master node to download and install the kubernetes dashboard

```bash
GITHUB_URL=https://github.com/kubernetes/dashboard/releases
VERSION_KUBE_DASHBOARD=$(curl -w '%{url_effective}' -I -L -s -S ${GITHUB_URL}/latest -o /dev/null | sed -e 's|.*/||')
sudo k3s kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/${VERSION_KUBE_DASHBOARD}/aio/deploy/recommended.yaml
```

## Install kubectl | LOCAL PC

The dashboard UI is accessible only from localhost. To access it from the browser you need to install kubectl and copy the cluster configuration to your local machine.

ARTICLE! https://stackoverflow.com/questions/50602172/kubectl-multiple-net-http-tls-handshake-timeout

Follow the installation steps [here](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

## Copy the master node config file | MASTER NODE

On K3S has the default configuration file is inside /etc/ancher/k3s/
[forum post](https://stackoverflow.com/questions/68287330/deleted-kube-config)

The directory .kube might not be present in the system, if so you need to create it first

Inside the master node run:

```bash
mkdir ~/.kube
```

Copy the configuration file inside the .kube folder

Inside the master node run:

```bash
cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
```

Next you need to copy the configuration file ~/.kube/config from your master node to your local pc

Inside the master node run the following command and copy the output [ctrl+c]:

```bash
cat ~/.kube/config
```

## Copy the configuration file | LOCAL PC

Take the .kube/config file and copy its content inside your local pc.
Create the .kube folder first if it doesn't exist

```bash
mkdir ~/.kube
cd ~/.kube
sudo nano config
// PASTE the config file contents, salve and close
```

## create service account | LOCAL PC

Create a service account with name <name> (you can put whatever you want, but from my experience is better if you use the same account name that you use to log in in your machine, where you import the .kube directory) in namespace kube-system

inside my_user.yaml
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
name: <your account user_name>
namespace: kube-system
```

## Create the cluster role association | LOCAL PC

inside cluster-admin-role-association.yml

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
name: <your account user_name>
roleRef:
apiGroup: rbac.authorization.k8s.io
kind: ClusterRole
name: cluster-admin
subjects:
- kind: ServiceAccount
name: <your account user_name>
namespace: kube-system
```

## GET THE ACCESS TOKEN FORM THE DASHBOARD

```bash
sudo k3s kubectl create -f dashboard.admin-user.yml -f dashboard.admin-user-role.yml
sudo k3s kubectl -n kubernetes-dashboard describe secret admin-user-token | grep '^token'
```

Output Token Cluster:

```text
eyJhbGciOiJSUzI1NiIsImtpZCI6ImpwRFUxMkYxbzZEUFE0T0ZHSnJEdWNaNldFVWFmZ3BhYWRRUENLTWV1T2MifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLWMya2pwIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiI0YjAwY2M2OC1iNWUzLTQ1ODEtOTFkNy1hMTk0MTk4ZjQ2NjciLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.
do8DU0dhhr1ABrh_hzH_JV3Cw1zk7Ax5kYRSUiDEPr4LRaZVGG3XnS1UpEDtcuWIkQe_I_XxqRhqi_sT2I1n4jHbRTcXTxoDlqjdy8YNYL4hSqDfxNs6bSzJCFm_MN4Zq-topUgCRlXXme7Q3ShKOPxvSueNnd3WD0ko3eXxPz4JG2EVeb0dBAGrotx26qPhv1hLlYaHmxzT5yqpont545Hb6pjwm21feQYnx8D9LFIGLx4TWjm8Wg_QvpFGRmB2T17UqnDSdH8CQOuA2vVnnRdSVEVA3NsioMIxzCz24hLq_ekCGG9NUtD3mGdNd-PkAigZ-0sh8noSLjZdgcEX1w
```

You need this toket to access the UI

## Start the dashboard UI | LOCAL PC

Start the dashboard ui in the browser by using a proxy

```bash
sudo kubectl proxy
```

Visit the url below in the browser
http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login