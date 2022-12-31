## Pod Example: (w/ Binding object)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: nginx
spec:
  Containers:
  - name: nginx
    image: nginx
    ports:
      - containerPort: 8080
  nodeName: node02 # SET THIS TO NAME OF NODE
```

## Binding Object Example: (If no scheduler)
```yaml
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node02
```
- Send a post request: `curl --header "Content-Type:application/json" --request POST --data '{"apiVersion":"v1", "kind": "Binding" ... }' http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/`

## Manual Scheuling:
- `kubectl apply -f nginx.yaml`
- `kubectl replace --force -f nginx.yaml`