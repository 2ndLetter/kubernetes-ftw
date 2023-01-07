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
- `kubectl...`




    1  kubectl get no -A
    2  kubectl get pods -A
    3  k describe -n kube-system kube-apiserver-controlplane
    4  k describe -n kube-system pod  kube-apiserver-controlplane
    5  k describe -n kube-system pod  kube-apiserver-controlplane
    6  kubectl get pod -A
    7  kubectl describe -n kube-system pod etcd-controlplane 
    8  k describe -n kube-system pod  kube-apiserver-controlplane
    9  k describe -n kube-system pod  kube-apiserver-controlplane | grep .crt
   10  k describe -n kube-system pod  kube-apiserver-controlplane | grep .crt
   11  openssl x509 -in /etc/kubernetes/pki/apiserver.crt
   12  openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
   13  openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
   14  openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout | grep -i common
   15  openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout | grep -i CN
   16  openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
   17  kubectl describe -n kube-system pod etcd-controlplane 
   18  stat /etc/kubernetes/pki/etcd/server.crt
   19  stat /etc/kubernetes/pki/etcd/server.crt
   20  openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text -noout
   21  openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text -noout
   22  openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
   23  openssl x509 -in /etc/kubernetes/pki/ca.crt -text -noout
   24  cat /etc/kubernetes/manifests/etcd.yaml 
   25  vim /etc/kubernetes/manifests/etcd.yaml 
   26  vim notes
   27  cat notes 
   28  cat notes | grep crt
   29  cat notes | grep crt
   30  ls /etc/kubernetes/pki/etcd
   31  vim /etc/kubernetes/manifests/etcd.yaml 
   32  k get pod -A
   33  kubectl logs -n kube-system pod kube-apiserver-controlplane
   34  kubectl logs -n kube-apiserver-controlplane
   35  kubectl logs kube-apiserver-controlplane
   36  kubectl logs etcd-controlplane
   37  kubectl get pod
   38  kubectl get all -A
   39  docker ps -a
   40  crictl ps -a
   41  k describe -n kube-system pod etcd-controlplane 
   42  k logs etcd-controlplane 
   43  crictl ps -a
   44  crictl logs etcd
   45  crictl logs etcdd
   46  crictl logs 1344c693f7e1b
   47  crictl logs 1344c693f7e1b -f
   48  crictl logs 1344c693f7e1b | grep -i error
   49  crictl logs 1344c693f7e1b | grep -i failed
   50  crictl logs 1344c693f7e1b
   51  crictl logs 1344c693f7e1b
   52  crictl logs 1344c693f7e1b
   53  vim /etc/kubernetes/manifests/etcd.yaml 
   54  k get all -A
   55  crictl ps -a
   56  critctl logs c03a126e8a1c8
   57  crictl logs c03a126e8a1c8
   58  crictl logs c03a126e8a1c8
   59  cd /etc/kubernetes/manifests/
   60  ls
   61  vim kube-apiserver.yaml 
   62  openssl x509 -in /etc/kubernetes/pki/ca.crt -text -noout
   63  vim kube-apiserver.yaml 
   64  ls /etc/kubernetes/pki/etcd/
   65  vim kube-apiserver.yaml 
   66  crictl ps -a
   67  k get pod -a
   68  k get pod -A
   69  history