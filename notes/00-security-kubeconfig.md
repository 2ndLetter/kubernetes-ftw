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
- Its better to use Full Paths to certificates
- Instead of "certificate-authority", you can use "certificate-authority-data" and the base64 encoded output
- Context names are arbitrary, they can be named anything

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
    certificate-authority: /etc/kubernetes/pki/ca.crt
    server: https://my-kube-playground:6443
- name: production
  ...omitted...

contexts:
- name: my-kube-admin@my-kube-playground
  context:
    user: my-kube-admin
    cluster: my-kube-playground
    namespace: finance # <------ Optional default namespace when in this context
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
- `cat .kube/config`


- `kubectl config use-context -h`
- `kubectl config --kubeconfig=/root/my-kube-config use-context research`
- `kubectl config --kubeconfig=/root/my-kube-config current-context`
- Copy contents of my-kube-config into .kube/config to make the current settings the default
