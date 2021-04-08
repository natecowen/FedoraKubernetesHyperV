# VM Installation Steps

**Follow the steps below for each node/server in the cluster**

[Download Fedora ISO Here](https://getfedora.org/en/server/download/)

## Setup VM In Hyper V

**Add a new VM via Hyper-V:** ensure it has the following: 
- Specify Name: FedoraServer33-* (ex FedoraServer33-Master)
- Select Generation 1
- Assign 4096MB Memory
- Select KubernetesSwitch (see how to setup in Readme.md)
- Create Virtual Hard Disk
- Select Install an Operating system from a bootable image (the .ISO downloaded from Fedora)
- Click Finish & then right click the VM and select settings
- Change Processors to 2 virtual processors
- Select the network adapter and click advanced features
  - set the static Mac Address
  - configure your router's DNS to set static IP address for that Mac Address
- Start the VM and work through Fedora Install

---

**Install Fedora** and ensure that you set the following items up in the installation menus: 
- Set static ip, MAC Address, and hostname during installation
- Set TimeZone
- Set Users
- Reboot After Installation is complete

--- 

## Setup Fedora for K8s

### Disable ZRAM

Use an internet browswer and log into the web client at x.x.x.160:9090. Select "Services" from the left hand menu. Find or Filter Services by "swap-create@zram0.service". Select the service and click the 3 dots next to the service name. Select mask from the drop down menu to permanently disable.

> Note: If using a different version of fedora or linux, youu may need to disable swap memory. Do that by following the steps below

```shell
# temporarily disable swap 
sudo swapoff -a

# permanently turn off swap 
cd /etc/fstab
sudo nano fstab
# comment out any line that has swap in it.
# reboot
shutdown -r now 
# verify no swap devices are listed 
lsblk 
free 
```
> Helpful Links:  [Enabling Swaps](https://www.techrepublic.com/article/how-to-enable-the-zram-module-for-faster-swapping-on-linux/). [CoreOs/Fedora thread discussing k8s and zram](https://github.com/coreos/fedora-coreos-tracker/issues/509). 

> Confused to why we disable swap? See the following: [Formus Post](https://discuss.kubernetes.io/t/swap-off-why-is-it-necessary/6879) [Article](https://medium.com/tailwinds-navigator/kubernetes-tip-why-disable-swap-on-linux-3505f0250263) [Github issue requesting K8's to work with swap](https://github.com/kubernetes/kubernetes/issues/53533)

---

### Verify the following pre-requisites for Kubernetes

**Directly taken from Kubernetes documentation**: [see full documentation here](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/): 

```shell
# verify that each machine has a unique mac address and product_uuid 
ifconfig -a
sudo cat /sys/class/dmi/id/product_uuid

# Let iptables see bridged traffic 
sudo modprobe br_netfilter

# Ensure netbridge.bridge-nf-call-iptables is set to 1
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```

---

### Install DNF Packages

```shell
sudo dnf  -y install  iproute-tc git htop

```

> You may need to setup a proxy for DNF. This is required for some corporate networks.
>  - setup proxy via: 
>    - ```sudo nano /etc/dnf/dnf.conf``` adding  ```proxy=http://proxyserver:port```
>    - see [Setting Up System Wide Proxy](https://computingforgeeks.com/configure-system-wide-proxy-settings-on-centos-rhel-fedora/)


--- 

### Add Port Rules for  rules to the firewalld

```shell 
sudo firewall-cmd --permanent --zone=public --add-port=6643/tcp
sudo firewall-cmd --permanent --zone=public --add-port=2379/tcp
sudo firewall-cmd --permanent --zone=public --add-port=2380/tcp
sudo firewall-cmd --permanent --zone=public --add-port=10250/tcp
sudo firewall-cmd --permanent --zone=public --add-port=10251/tcp
sudo firewall-cmd --permanent --zone=public --add-port=10252/tcp

# or disable firewalld (not recommended only for troubleshooting/testing purposes)
# sudo systemctl stop firewalld 
# sudo systemctl disable firewalld 
# sudo shutdown -r now 
# verify it stays disabled after reboot via sudo systemctl status firewalld
```


---

## Install Docker


Follow full instructions here: [Install Docker Engine on Fedora](https://docs.docker.com/engine/install/fedora/)

Verify old docker engine was not included with DNF Package Manager by using ```dnf list installed "docker*"```.  If installed you will need to remove those older docker packages and their associated depenedencies. More info in the full instructions above. 

### Setup the Docker Repository for Installation:
Run the following commands to setup the docker repository
```shell
 sudo dnf -y install dnf-plugins-core

 sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
```

### Install the Docker Engine
```shell
 sudo dnf install docker-ce docker-ce-cli containerd.io
```
> **If prompted to accept the GPG key**, verify that the fingerprint matches 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

After installation, run the following commands: 
```shell
# Start Docker 
sudo systemctl start docker
#Verify docker is started (should return docker version)
docker -v
# Verify we can run a docker image
sudo docker run hello-world
```

### Configure Docker Group and Assign Users

```shell
# create the group called docker
sudo groupadd docker
# add users to the group (ncowen being the username)
sudo usermod -aG docker ncowen
# have the user logout/login and try running a docker command (without sudo)
docker -v
```

> Note that docker group grants privileges similar to root for that user. This can be less than desirable for some users. 

### Configure Docker to Start on Boot

```shell
 sudo systemctl enable docker.service
 sudo systemctl enable containerd.service
```

---

## Install Certificates for Other Servers/URIs (If Required)

> This will be used for Docker/Docker Registry once we get it setup. 

- (CA Cert Redhat)[https://www.redhat.com/sysadmin/ca-certificates-cli#:~:text=A%20package%20included%20with%20many%20distributions,- %20including%20Red,the%20same%20well-known%20CA%20certificates%20found%20in%20Firefox.]
(CA Cert Fedora)[https://docs.fedoraproject.org/en-US/quick-docs/using-shared-system-certificates/]


