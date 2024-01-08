# K8s 101

## Episode 1 (getting started with minikube)

[youtube](https://www.youtube.com/watch?v=IcslsH7OoYo&list=PL2_OBreMn7FoYmfx27iSwocotjiikS5BD&index=3&t=8s&ab_channel=JeffGeerling)

### steps

1. Install minikube
2. Create a cluster
3. Create a deployment
4. Expose it
5. Run the service

```bash
# Show all nodes of the cluster
kubectl get nodes
# Create a deployment
kubectl create deployment <name> --image=<dockerImage>
# Expose the deployment to access the application
kubectl expose deployment <deploymentName> --type=NodePort --port=80
# Get the running service (services are created once the deployment is exposed) it will output the cluser ip and ports number
kubectl get services <deploymentName> 
# Get the ip address (example with minikube)
minibuke ip
# Or let minikube serve the application
minikube service <deploymentName>
#edit deployment
sudo k0s kubectl edit deployment <deploymentName>

```

To turn off the cluster or delete it you can run

```bash
## turn off the cluster
minikube halt
## delete it
minikube delete

```
