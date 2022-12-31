# basic-pod:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp      # CAN BE USED TO LINK POD TO SERVICE
    tier: front-end # CAN BE USED TO LINK POD TO SERVICE
  namespace: <namespace_name>
spec:
  containers:
  - name: nginx-container
    image: nginx
```

## PODs:
- `kubectl get pods -o wide`
- `kubectl run <pod_name> --image=<image_name>`
- `kubectl describe pod <pod_name>`
- `kubectl delete pod <pod_name>`
- `kubectl run redis --image=redis --dry-run=client -o yaml > redis.yml`
  - Manually update file as required (via vim)
  - `kubectl create -f redis.yml`