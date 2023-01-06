# View Certificate Details:
## Documentation:
- https://kubernetes.io/docs/home/
---

## Commands/Notes:
- When trying to analyse the TLS configuations, find out how the cluster was setup:
  - The Hard Way:
    - Manually generated certificates yourself
    - Deployed as an installed service
  - kubeadm:
    - Automatically generated certificates by the tool (kubeadm)
    - Deployed as a Pod
- Retrieve information:
  - For kubeadm, look at the /etc/kubernetes/manifests/kube-apiserver.yaml:
```yaml
spec:
  containers:
  - command:
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
    - --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
    - --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
    - --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
    - --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelete-client.key
    - --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
    - --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
```
- Example certificate inspection (/etc/kubernetes/pki/apiserver.crt):
- `openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout`
```
Certificate:
  Data:
    ...
    Issuer: CN=kubernetes
    Validity
      Not Before: Feb 11 05:39:19 2019 GMT
      Not After : Feb 11 05:39:20 2020 GMT
    Subject: CN=kube-apiserver
    ...
    X509v3 extensions:
      X509v3 Subject Alternative Name:
        DNS:master, DNS:kubernetes, DNS:kubernetes.default, DNS:kubernetes.default.svc, DNS:kubernetes.default.svc.cluster.local, IP Address:10.96.0.1, IP Address:172.17.0.27
```



---

