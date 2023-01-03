# OS Upgrades:
## Documentation:
- https://kubernetes.io/docs/home/

## Usage:
- Imperative Way:
  - `kubectl ...`
- Delcarative Way:
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
## Commands:
- `kube-controller-manager --pod-eviction-timeout=5m0s ...`
- `kubectl drain node-1` # evict pods and cordon the node, which are deployed on other nodes
- `kubectl uncordon node-1` # Pods *can* be deployed to node-1 again
- `kubectl cordon node-1` # Prevents new pods from being scheduled on the node

## Notes:
- TBD

## KodeKloud Lab:
- `kubectl get pods -l app=blue -o wide`
- `kubectl drain node01 --ignore-daemonsets`
- `kubectl uncordon node01`
- `kubectl describe node controlplane | grep Taint`
- `kubectl drain node01 --ignore-daemonsets --force`
- `kubectl cordon node01`
