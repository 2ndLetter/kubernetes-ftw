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