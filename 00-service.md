## Service (NodePort) Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:              # THIS IS AN ARRAY
    - targetPort: 80  # POD PORT (OPTIONAL)
      port: 80        # SERVICE PORT
      nodePort: 30008 # NODE PORT, RANGE 30000 - 32767 (OPTIONAL)
  selector:
    app: myapp        # LINKS SERVICE TO POD
    type: front-end   # LINKS SERVICE TO POD
```
- `kubectl create -f service-nodeport-definition.yml`
- `kubectl get services`
- `curl http://<node_ip>:30008`

## Service (ClusterIP) Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP   # DEFAULT TYPE
  ports:
    - targetPort: 80
      port: 80
  selector:
    app: myapp      # LINKS SERVICE TO POD
    type: front-end # LINKS SERVICE TO POD
```
- `kubectl create -f service-clusterip-definition.yml`
- `kubectl get services`
- `curl http://<cluster_ip>`
- `curl http://backend-end`

## Service (LoadBalancer) Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
```