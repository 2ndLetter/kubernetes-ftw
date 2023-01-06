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
- `cat /etc/kubernetes/manifests/kube-apiserver.yaml`
- 
- `cat akshay.csr | base64 -w 0`
- Create akshay.yaml:
```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: akshay
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQXY4azZTTE9HVzcrV3JwUUhITnI2TGFROTJhVmQ1blNLajR6UEhsNUlJYVdlCmJ4RU9JYkNmRkhKKzlIOE1RaS9hbCswcEkwR2xpYnlmTXozL2lGSWF3eGVXNFA3bDJjK1g0L0lqOXZQVC9jU3UKMDAya2ZvV0xUUkpQbWtKaVVuQTRpSGxZNDdmYkpQZDhIRGFuWHM3bnFoenVvTnZLbWhwL2twZUVvaHd5MFRVMAo5bzdvcjJWb1hWZTVyUnNoMms4dzV2TlVPL3BBdEk4VkRydUhCYzRxaHM3MDI1ZTZTUXFDeHUyOHNhTDh1blJQCkR6V2ZsNVpLcTVpdlJNeFQrcUo0UGpBL2pHV2d6QVliL1hDQXRrRVJyNlMwak9XaEw1Q0ErVU1BQmd5a1c5emQKTmlXbnJZUEdqVWh1WjZBeWJ1VzMxMjRqdlFvbndRRUprNEdoayt2SU53SURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBQi94dDZ2d2EweWZHZFpKZ1k2ZDRUZEFtN2ZiTHRqUE15OHByTi9WZEdxN25oVDNUUE5zCjEwRFFaVGN6T21hTjVTZmpTaVAvaDRZQzQ0QjhFMll5Szg4Z2lDaUVEWDNlaDFYZnB3bnlJMVBDVE1mYys3cWUKMkJZTGJWSitRY040MDU4YituK24wMy9oVkN4L1VRRFhvc2w4Z2hOaHhGck9zRUtuVExiWHRsK29jQ0RtN3I3UwpUYTFkbWtFWCtWUnFJYXFGSDd1dDJveHgxcHdCdnJEeGUvV2cybXNqdHJZUXJ3eDJmQnErQ2Z1dm1sVS9rME4rCml3MEFjbVJsMy9veTdqR3ptMXdqdTJvNG4zSDNKQ25SbE41SnIyQkZTcFVQU3dCL1lUZ1ZobHVMNmwwRERxS3MKNTdYcEYxcjZWdmJmbTRldkhDNnJCSnNiZmI2ZU1KejZPMUU9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```
  - `kubectl apply -f akshay.yaml`
  - `kubectl get csr`
  - `kubectl certificate approve akshay`
  - `kubectl get csr`
  - `kubectl get csr agent-smith -o yaml`
  - `kubectl certificate deny agent-smith`
  - `kubectl delete csr agent-smith`
