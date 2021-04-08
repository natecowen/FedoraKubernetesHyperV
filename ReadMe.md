# Kubernetes Setup for Fedora

**Overview:** This document outlines the required steps to install Kubernetes on Fedora. This will be a starting point for our a Hyper-V Kubernetes Cluster Setup.  Documentation will be broken up into several mark-down files for clarity.

---

## VM Information
| Node IP     	| Type   	| OS     	| RAM   	| CPU     	|
|-------------	|--------	|--------	|--------	|---------- |
| x.x.x.160   	| Master 	| Fedora 	| 4096MB  | 2 Virtual |
| x.x.x.161    	| Child  	| Fedora 	| 4096MB  | 2 Virtual |


> Setup requires static MAC DNS Address and Router Configuration to Map those DNS Entries to a static IP

[Follow the steps 1 & 3 here create a virtual switch](http://vijayshinva.github.io/kubernetes/2018/07/28/setting-up-a-kubernetes-cluster-on-a-windows-laptop-using-hyper-v.html#:~:text=Setting%20up%20a%20Kubernetes%20Cluster%20on%20a%20Windows,that%20you%20copied%20earlier%20from%20the%20Master%20node.)


---

## Steps to Follow

- [VM SETUP](./Documentation/01-VM-Setup.md)
- [KUBERNETES INSTALL](./Documentation/02-Kubernetes-Installation.md)
- [SETTING UP LOCAL MACHINE](./Documentation/03-Setup-Local-Machine.md)
- [TROUBLESHOOTING](./Documentation/04-Troubleshooting.md)




