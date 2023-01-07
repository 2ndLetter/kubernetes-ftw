# KubeConfig:
## Documentation:
- https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/

## Notes/Commands:
- `kubectl get pods --kubeconfig config`
- KubeConfig file default location: $HOME/.kube/config
- The config file does NOT create a Kubernetes object
- `kubectl config -h`
- `kubectl config view`
- `kubectl config view --kubeconfig=my-custom-config`
- `kubectl config use-context prod-user@production`

## Usage:
- Imperative Way:
  - `kubectl ...`
- Delcarative Way:
  - config:
```yaml
apiVersion: v1
kind: Config

current-context: my-kube-admin@my-kube-playground

clusters:
- name: my-kube-playground
  cluster:
    certificate-authority:
    server: https://my-kube-playground:6443
- name: production
  ...omitted...

contexts:
- name: my-kube-admin@my-kube-playground
  context:
    user: my-kube-admin
    cluster: my-kube-playground
- name: prod-user@production
  ...omitted...

users:
- name: my-kube-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
- name: prod-user
  ...omitted...
```


## KodeKloud Lab:
- `kubectl ...`
- `kubectl ... --dry-run=client -o yaml > template.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```
- `kubectl apply -f template.yaml`
