# Init Containers:

## Documentation:
- tbd

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: simple-webaps
spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
      - containerPort: 8080
  - name: log-agent
    image: log-agent
```

## KodeKloud Lab:
- `kubectl run yellow --image=busybox --dry-run=client -o yaml --command -- sleep 1000 > yellow.yaml`
- Add second container to yellow.yaml via VIM
- `kubectl apply -f yellow.yaml`
- `kubectl -n elastic-stack logs kibana -f`
- `kubectl get po app -n elastic-stack -o yaml > app.yaml`
- `kubectl replace --force -f app.yaml`