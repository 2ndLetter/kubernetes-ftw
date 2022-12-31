
___


















## Resource Requests - PODs: [documentation](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#example-1)
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
