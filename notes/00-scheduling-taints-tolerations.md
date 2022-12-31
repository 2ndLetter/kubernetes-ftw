## Tolerations - PODs:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```

## Taints and Tolerations:
- `kubectl taint nodes <node-name> <key=value>:<taint-effect>` # Example syntax
  - taint-effect: NoSchedule | PreferNoSchedule | NoExecute
- `kubectl taint nodes node1 app=blue:NoSchedule`
- `kubectl describe node kubemaster | grep Taint` # Review taints on the Master node
- `kubectl taint nodes node01 spray=mortein:NoSchedule`
- `kubectl run bee --image=nginx --dry-run=client -o yaml > bee.yaml`
  - Add "tolerations" section
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        run: bee
      name: bee
    spec:
      containers:
      - image: nginx
        name: bee
      tolerations:
      - key: "spray"
        operator: "Equal"
        value: "mortein"
        effect: "NoSchedule"  
    ```
  - `kubectl apply -f bee.yaml`
  - `kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-`
    - This removes the default taint on the controlplane node