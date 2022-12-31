# Manifest File Notes & Examples
___
### apiVersion (string): Specifies the API version to use
### kind (string): Specifies the type of resource you want to create
### metadata (dict): Specifies data about the object (ie. Name, Labels, etc.)
### spec (dict): Specification of the desired behavior of the object
___






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

## Labels | Selectors | Annotations:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    app: App1           # LABEL
    function: Front-end # LABEL
```
- `kubectl get pods --selector app-App1` # Returns object based on label

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metdata:
  name: simple-webapp
  labels:                   # REPLICASET OBJECT LABEL
    app: App1               # REPLICASET OBJECT LABEL
    function: Front-end     # REPLICASET OBJECT LABEL
  annotations:
    buildversion: 1.34      # METADATA
spec:
  replicas: 3
  selector:                 # CONNECTS REPLICASET TO POD
    matchLabels:            # CONNECTS REPLICASET TO POD
      app: App1             # CONNECTS REPLICASET TO POD
  template:
    metadata:
      labels:               # CONNECTS REPLICASET TO POD
        app: App1           # CONNECTS REPLICASET TO POD
        function: Front-end
    spec:
      containers:
      - name: simple-webapp
        image: simple-webapp
```

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

## Node Selectors - PODs:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: data-processor
    image: data-processor
  nodeSelector:
    size: Large # LABELS ASSIGNED TO NODES
```
- `kubectl label nodes <node-name> <label-key>=<label-value>` # LABEL COMMAND EXAMPLE
- `kubectl label nodes node-1 size=Large` # RUN THIS COMMAND BEFORE DEPLOYING THE ABOVE POD

## Node Affinity - PODs: [documentation](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: data-processor
spec:
  containers:
  - name: data-processor
    image: data-processor
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In | NotIn | Exists
            values:
            - Large
            - Medium
```

## Resource Requests - PODs: [documenation](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#example-1)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-web-color
  labels:
    name: simple-web-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    resources:
      requests:
        memory: "1Gi"
        cpu: 1
```

## Limit Range Example (Memory): [documentation](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/)
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

## Limit Range Example (CPU): [documentation](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/)
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```

## DaemonSet Example: [docs](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: myapp-daemon
spec:
  selector:
    matchLabels:
      app: monitoring-agent # ENSURE THIS LABEL MATCHES THE POD TEMPLATE
  template:
    metadata:
      labels:
        app: monitoring-agent
    spec:
      containers:
      - name: monitoring-agent
        image: monitoring-agent  
```
- `kubectl get daemonsets`

## Schedulers:
my-scheduler-config.yaml:
```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler
leaderElection:
  leaderElect: true # MULTI-MASTER LEADER ELECTION
  resourceNamespace: kube-system
  resourceName: lock-object-my-scheduler
```
my-custom-scheduler.yaml:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --config=/etc/kubernetes/my-scheduler-config.yaml
    image: k8s.grc.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```
pod-definition.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  schedulerName: my-custom-scheduler
```
- `kubectl get events -o wide` # See events in the current namespace
- `kubectl logs my-custom-scheduler --name-space=kube-system`

## Multi Profile Scheduler
```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler-2
  plugins:
    score:
      disabled:
        - name: TaintToleration
        enabled:
        - name: MyCustomPluginA
        - name: MyCustomPluginB
- schedulerName: my-scheduler-3
  plugins:
    preScore:
      disabled:
        - name: '*'
    score:
      disabled:
        - name: '*'
```
## Deployment Update Strategy Example:
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
        tier: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      tier: front-end
  strategy:
    type: RollingUpdate | Recreate
    rollingUpdate:        # USE IF TYPE IS RollingUpdate
      maxSurge: 25%       # USE IF TYPE IS RollingUpdate
      maxUnavailable: 25% # USE IF TYPE IS RollingUpdate
```

## ENV Variables - PODs:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
spec:
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
  env:                # PLAIN KEY VALUE
    - name: APP_COLOR
      value: pink
```
```yaml
env:                  # CONFIGMAP
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef:
```
```yaml
env:                  # SECRETS
  - name: APP_COLOR
    valueFrom:
      secretKeyRef:
```
- `docker run -e APP_COLOR=pink simple-webapp-color` # DOCKER RUN WAY
