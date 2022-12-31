## Deployment Example:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        tier: front-end # THIS MUST MATCH SO THE RS CAN MANAGE THE PODS
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      tier: front-end   # THIS MUST MATCH SO THE RS CAN MANAGE THE PODS
```

## Deployments:
- `kubectl get all`
- `kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yml`
- `kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yml`