# Manifest File Notes & Examples
___
### apiVersion (string): Specifies the API version to use
### kind (string): Specifies the type of resource you want to create
### metadata (dict): Specifies data about the object (ie. Name, Labels, etc.)
### spec (dict): Specification of the desired behavior of the object
___

## Minimal Example:
```yaml
apiVersion: v1 | apps/v1
kind: Pod | Service | ReplicaSet | Deployment
metadata:
  name: <name_of_object>
  labels:
    <key>: <value>
    <key>: <value>
spec:
  <object_specific_item>
    <key>: <value>
    <key>: <value>
```

## Pod Example:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp # CAN BE USED TO LINK POD TO SERVICE
    tier: front-end # CAN BE USED TO LINK POD TO SERVICE
  namespace: <namespace_name>
spec:
  containers:
  - name: nginx-container
    image: nginx
```

## ReplicationController Example:
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        tier: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
```

## ReplicaSet Example:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
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
      type: front-end # THIS MUST MATCH SO THE RS CAN MANAGE THE PODS
```

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
      type: front-end # THIS MUST MATCH SO THE RS CAN MANAGE THE PODS
```

## Service (NodePort) Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports: # THIS IS AN ARRAY
    - targetPort: 80 # POD PORT (OPTIONAL)
      port: 80 # SERVICE PORT
      nodePort: 30008 # NODE PORT, RANGE 30000 - 32767 (OPTIONAL)
  selector:
    app: myapp # LINKS SERVICE TO POD
    type: front-end # LINKS SERVICE TO POD
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
  type: ClusterIP # DEFAULT TYPE
  ports:
    - targetPort: 80
      port: 80
  selector:
    app: myapp # LINKS SERVICE TO POD
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

## Namespace Example:
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

## ResourceQuota Example: 
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

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