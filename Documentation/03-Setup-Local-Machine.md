# Setting Up Your Local Machine for Kubectl

Quick and Dirty setup, should be a bit more robust for actual corporate use

- Follow one of the steps to [install Kubectl here](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
- Download and install something like Linux for Windows Subsystems or just use the new [windows terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab). These will allow you to run linux commands
- On your master node view your ~.kube/config with `cat ~.kube/config`.
- Copy the contents and paste them into your Windows 10 .kube/config file
- Restart your command line and test out via `kubectl get nodes`
- 