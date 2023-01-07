# View Certificate Details:
## Documentation:
- https://kubernetes.io/docs/home/
- https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/
- https://github.com/mmumshad/kubernetes-the-hard-way/tree/master/tools
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
```yaml
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
  - Follow the same steps to inspect the other certificates
  - What to look for:
    - CN Name (corrrect name)
    - ALT Names (includes all names)
    - Organization
    - Issuer (not self)
    - Expiration (not expired)
  - If you run into issues, Inspect Service Logs:
    - If The Hard Way If stood up The Hard Way:
      - `journalctl -u etcd.service -l`
      - Look for "failed" and "error"
      - Look for incorrect paths to certificates
    - If kubeadm:
      - `kubectl logs etcd-master`
    - If kube-apiserver or etcd-server is down, you need to inspect the docker logs:
      - `docker ps -a`
      - `docker logs <container_id>`  
---

## KodeKloud Lab:
- `kubectl get pods -A`
- `k describe -n kube-system pod  kube-apiserver-controlplane`
- `kubectl describe -n kube-system pod etcd-controlplane `
- `k describe -n kube-system pod  kube-apiserver-controlplane | grep .crt`
- `openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout`
- `openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text -noout`
- `openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text -noout`
- `openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout`
- `openssl x509 -in /etc/kubernetes/pki/ca.crt -text -noout`
- `vim /etc/kubernetes/manifests/etcd.yaml `
- `vim notes`
- `cat notes | grep crt`
- `ls /etc/kubernetes/pki/etcd`
- `vim /etc/kubernetes/manifests/etcd.yaml `
- `k get pod -A`
- `kubectl logs -n kube-system pod kube-apiserver-controlplane`
- `kubectl logs -n kube-apiserver-controlplane`
- `kubectl logs kube-apiserver-controlplane`
- `kubectl logs etcd-controlplane`
- `crictl ps -a`
- `crictl logs 1344c693f7e1b`
- `vim /etc/kubernetes/manifests/etcd.yaml `
- `crictl ps -a`
- `crictl logs c03a126e8a1c8`
- Error: "tcp 127.0.0.1:2379: connect: connection refused"
  - Port 2379 is ETCD
- `vim kube-apiserver.yaml `
- `openssl x509 -in /etc/kubernetes/pki/ca.crt -text -noout`
- `vim kube-apiserver.yaml `
- `crictl ps -a`
- `k get pod -A`
