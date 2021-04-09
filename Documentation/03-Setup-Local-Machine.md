# Setting Up Your Local Machine for Kubectl

Quick and Dirty setup, should be a bit more robust for actual corporate use

- Follow one of the steps to [install Kubectl here](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
- Download and install something like Linux for Windows Subsystems or just use the new [windows terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab). These will allow you to run linux commands
- On your master node view your ~.kube/config with `cat ~.kube/config`.
- Copy the contents and paste them into your Windows 10 .kube/config file
- Restart your command line and test out via `kubectl get nodes`

---

## Testing K8's

To test deployments within your K8's Cluster. 
- Copy the YAML documents with in the yaml folder to your `~/.kube` folder (preferably in their own subdirectory).
- Update the `service-nginx.yaml` file to have your child clusters IP address in the "external-ips" field. 
- Run either `kubectl apply -f ./directory name` or `kubectl apply -f filename.yaml` to each file. 
- Verify the services, deployments, and namespace exists with 
  - `kubectl get nodes`
  - `kubectl get namespaces`
  - `kubectl get services -n naco-dev`
  - `kubectl get deployments -n naco-dev`
- Attempt to exec into the busy box pod with `kubectl exec -it busybox-guid sh`. Run a ping or traceroute to verifiy internet connectivity from the nodeport service
- Attempt to connect to your nginx pod by opening a browser and entering in childNodeIPAddress:30091. You should be presented with the Nginx HTML Welcome Page