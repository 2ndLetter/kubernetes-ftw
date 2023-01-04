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
- `kubectl ...`
- `kubectl ... --dry-run=client -o yaml > template.yaml`
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
