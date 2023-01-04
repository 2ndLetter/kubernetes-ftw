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
- `k describe node | grep Taint`
- `k get pods -l app=blue`
- `kubectl drain controlplane --ignore-daemonsets`
- `k get pods -l app=blue`
- `cat /etc/*release*`
- `apt update`
- `apt-get install kubeadm=1.25.0-00`
- `kubeadm upgrade plan`
- `kubeadm upgrade apply v1.25.0`
- `apt install kubelet=1.25.0-00`
- `systemctl daemon-reload`
- `systemctl restart kubelet`
- `k get nodes`
- `kubectl uncordon controlplane`
- `k get nodes`
- `kubectl drain node01 --ignore-daemonsets`
- `ssh node01`
- `kubeadm version`
- `apt-get update`
- `apt-get install kubeadm=1.25.0-00`
- `kubeadm upgrade node`
- `apt-get install kubelet=1.25.0-00`
- `systemctl daemon-reload`
- `systemctl restart kubelet`
- `exit`
- `k get nodes -o wide`
- `kubectl uncordon node01`
