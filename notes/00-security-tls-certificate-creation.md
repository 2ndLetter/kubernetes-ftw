# TLS Certificate Creation:
## Documentation:
- https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/
- https://kubernetes.io/docs/tasks/administer-cluster/certificates/

## Commands:

### Generate Certificate for the Kubernetes Certificate Authority:
- Generate Private Key: `openssl genrsa -out ca.key 2048`
```
ca.key
```
- Create Certificate Signing Request: `openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr`
```
ca.csr
```
- Generate a Signed Certificate (self-signed by private key): `openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt`
```
ca.crt
```

### Generate Client Certificate:
- Generate Private Key: `openssl genrsa -out admin.key 2048`
```
admin.key
```
- Create Certificate Signing Request: `openssl req -new -key admin.key -subj "/CN=kube-admin" -out admin.csr`
```
admin.csr
```
- Generate Signed Certificate : `openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt`
```
admin.crt
```
- NOTE: Add group to Certificate Signing Request when you need to identify the user as a Admin:
  - `openssl req -new -key admin.key -subj "/CN=kube-admin/OU=system:masters" -out admin.csr`

### Systemctl Client Certificates:
- Client Certificates for system components must be prefixed with prefixed "SYSTEM":
  - SYSTEM:KUBE-SCHEDULER
  - SYSTEM:KUBE-CONTROLLER-MANAGER
  - SYSTEM:KUBE-PROXY

### Using the admin key/certificate:
- Rest API Call: `curl https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt`
```
{
  "kind": "PodList",
  "apiVersion": "v1",
  "metadata": {
    "selfLink": "/api/v1/pods",
  },
  "items": []
}
```
- kube-config.yaml:
```
apiVersion: v1
kind: Config
clusters:
- cluster:
    certificate-authority: ca.crt
    server: https://kube-apiserver:6443
  name: kubernetes
users:
- name: kubernetes-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

## Notes:

### Server Certificates:
- KUBE-API SERVER: #
  - apiserver.crt (cert)
  - apiserver.key (priv key)
  - apiserver-etcd-client.crt    # <---- ACCESSES THE ETCD SERVER
  - apiserver-etcd-client.key    # <---- ACCESSES THE ETCD SERVER
  - apiserver-kubelet-client.crt # <---- ACCESSES THE KUBELET SERVER
  - apiserver-kubelet-client.key # <---- ACCESSES THE KUBELET SERVER
- ETCD SERVER:
  - etcdserver.crt (cert)
  - etcdserver.key (priv key)
- KUBELET SERVER:
  - kubelet.crt (cert)
  - kubelet.key (priv key)
### Client Certificates:
- ADMIN: (kubectl REST API) # <--------- ACCESSES THE KUBE-API SERVER
  - admin.crt (cert)
  - admin.key (priv key)
- KUBE-SCHEDULER:           # <--------- ACCESSES THE KUBE-API SERVER
  - scheduler.crt (cert)
  - scheduler.key (priv key)
- KUBE-CONTROLLER-MANAGER:  # <--------- ACCESSES THE KUBE-API SERVER
  - controller-manager.crt (cert)
  - controller-manager.key (priv key)
- KUBE-PROXY:               # <--------- ACCESSES THE KUBE-API SERVER
  - kube-proxy.crt (cert)
  - kybe-proxy.key (priv key)
