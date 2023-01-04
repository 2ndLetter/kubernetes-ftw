# Cluster Upgrade:
## Documentation:
- https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
- https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/
- https://kubernetes.io/docs/tasks/administer-cluster/cluster-upgrade/

## Commands:
- Master Node:
  - `kubeadm upgrade plan`
  - `apt-get update`
  - `apt-get install -y kubeadm=1.12.0-00`
  - `kubeadm upgrade apply v1.12.0`
  - `kubectl get nodes` # This shows the version of the kubelet
  - `apt-get install -y kubelet=1.12.0-00`
  - `systemctl daemon-reload`
  - `systemctl restart kubelet`
- Worker Node: (rinse repeat for each worker node)
  - `kubectl drain node-1`
  - `ssh <node>`
  - `apt-get update`
  - `apt-get install -y kubeadm=1.12.0-00`
  - `apt-get install -y kubelet=1.12.0-00`
  - `kubeadm upgrade node` # THIS WAS USED IN THE LAB
  - `kubeadm upgrade node config --kubelet-version v1.12.0`
  - `systemctl restart kubelet`
  - `kubectl uncordon node-1`

## Notes:
- Strategy 1: All at once
- Stretegy 2: Rolling (using the same Nodes, "pets")
- strategy 3: Rolling (using new Nodes, "cattle")
- You have to upgrade kubeadm before using it to upgrade the cluster
- You have to manually upgrade the kubelet, kubeadm doesn't manage kubelet

## KodeKloud Lab:
- `kubeadm upgrade plan`
- `k get nodes -A`
- `k describe node controlplane | grep Taint`
- `k describe node node01 | grep Taint`
- `k get pods -l app=blue`
- `kubectl drain controlplane --ignore-daemonsets`
- ``
- ``
- ``
-  

    1  kubeadm upgrade plan
    2  k get nodes
    3  k get nodes -A
    4  k get nodes
    5  k describe node controlplane | grep Taint
    6  k describe node nod01 | grep Taint
    7  k describe node node01 | grep Taint
    8  k get deploy
    9  k get deploy -o wide
   10  k get pods -l app=blue
   11  k get pods -l app=blue -o wide
   12  kubeadm upgrade plan
   13  kubectl -n kube-system get cm kubeadm-config -o yaml
   14  kubectl -n kube-system get cm kubeadm-config -o yaml
   15  kubeadm upgrade plan
   16  kubeadm upgrade plan | grep -i remote
   17  kubeadm --help
   18  kubectl drain controlplane
   19  kubectl drain controlplane --ignore-daemonsets 
   20  kubectl get pods -l app=blue
   21  kubectl get pods -l app=blue -o wide
   22  apt-get upgrade kubelet
   23  apt-get upgrade kubeadm=1.25.0
   24  kubeadm upgrade plan
   25  apt-get search nano
   26  apt-get --help
   27  apt search kubeadm
   28  apt search kubeadm=1.25.0
   29  apt search kubeadm=1.25.0-00
   30  apt search kubeadm 1.25.0
   31  kubeadm upgrade plan
   32  kubeadm --version
   33  kubeadm --help
   34  kubeadm version
   35  apt update
   36  apt search kubeadm
   37  apt-get install kubeadm=1.25.0
   38  apt-get install kubeadm=1.25.0-00
   39  kubeadm version
   40  kubeadm upgrade plan
   41  kubeadm upgrade apply v1.25.0
   42  apt-get upgrade kubelet
   43  apt install kubelet=1.25.0-00
   44  k get nodes
   45  k get nodes
   46  systemctl daemon-reload
   47  systemctl restart kublet
   48  systemctl restart kubelet
   49  k get nodes
   50  kubectl uncordon controlplane 
   51  kubectl drain node01
   52  kubectl drain node01 --ignore-daemonsets
   53  ssh node01
   54  k get nodes
   55  k get nodes -o wide
   56  kubectl uncordon node01
   57  ssh node01
   58  history


    1  kubeadm version
    2  apt-get upgrade kubeadm=1.25.0-00
    3  apt-get upgrade kubeadm=1.25.0
    4  apt-get install kubeadm=1.25.0-00
    5  apt-get install kubeadm=1.25.0-00
    6  apt-get update
    7  apt-get install kubeadm=1.25.0-00
    8  kubeadm upgrade node
    9  apt-get install kubelet=1.25.0-00
   10  systemctl daemon-reload
   11  systemctl restart kubelet
   12  exit
   13  history
   14  exit
   15  history

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```
- `kubectl apply -f template.yaml`
