# Troubleshooting Linux and Kubernetes

```shell
# view kube-system pods
kubectl get pods -n kube-system

# if any are down try to view their logs 
kubectl get logs -n kube-system {pod-name-guid-goes-here}
#view logs 
journalctl -u kubelet



#### Disable SELinux (Only required if the steps in Kubectl doesn't work )
cd /etc/sysconfig/
sudo nano selinux
# set variable to disabled https://www.tecmint.com/disable-selinux-in-centos-rhel-fedora/#:~:text=Disable%20SELinux%20Temporarily.%201%20%23%20echo%200%20%3E,SELinux%20permanently%2C%20move%20to%20the%20next%20section.%20


# reset K8s
sudo kubeadm reset
sudo rm -rf /etc/kubernetes


```
> [INSTALL K8 BEHIND PROXY](https://stackoverflow.com/questions/45580788/how-to-install-kubernetes-cluster-behind-proxy-with-kubeadm)


## How to add role/label to node: 
 ```shell
 kubectl label node fedoraserver-clone node-role.kubernetes.io/worker=worker
 ```

---
 
##Helpful Links

- [Set Hostname](https://ostechnix.com/how-to-set-or-change-hostname-on-linux/)
- [Set Static IPAddr](https://www.systutorials.com/how-to-set-the-static-ip-address-using-cli-in-fedoracentos-linux/)
- [Set IPAddr for Eth0](https://danielmiessler.com/study/manually-set-ip-linux/#:~:text=Using%20ifconfig%201%20Set%20Your%20IP%20Address%20ifconfig,default%20gw%20192.168.1.1%203%20Set%20Your%20DNS%20Server)
- [Hyper-V Setting up K8's Cluster](http://vijayshinva.github.io/kubernetes/2018/07/28/setting-up-a-kubernetes-cluster-on-a-windows-laptop-using-hyper-v.html#:~:text=Setting%20up%20a%20Kubernetes%20Cluster%20on%20a%20Windows,that%20you%20copied%20earlier%20from%20the%20Master%20node.)
- [Setting up VM Error Logs](https://stackoverflow.com/questions/60434077/failed-to-update-node-lease-error-operation-cannot-be-fulfilled-on-leases-coor)
- [Install Kubernetes Fedora](https://www.linuxtechi.com/install-kubernetes-1-7-centos7-rhel7/)

