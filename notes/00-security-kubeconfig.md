# KubeConfig:
## Documentation:
- https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/

## Notes/Commands:
- `kubectl get pods --kubeconfig config`
- KubeConfig file default location: $HOME/.kube/config


## Usage:
- Imperative Way:
  - `kubectl ...`
- Delcarative Way:
```yaml
apiVersion: v1
kind: Config

clusters:
- name: my-kube-playground
  cluster:
    certificate-authority:
    server: https://my-kube-playground:6443
  
contexts:
- context:
  name: development

users:
- name: developer
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
