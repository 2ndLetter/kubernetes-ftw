# TLS Certificate Creation:
## Documentation:
- https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/
- https://kubernetes.io/docs/tasks/administer-cluster/certificates/
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#securing-etcd-clusters

## Commands/Notes:

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

### System Client Certificates:
- Client Certificates for system components must be prefixed with prefixed "SYSTEM":
  - SYSTEM:KUBE-SCHEDULER
  - SYSTEM:KUBE-CONTROLLER-MANAGER
  - SYSTEM:KUBE-PROXY

### Client Certificate Notes:
- When Clients use their certificates, they need a copy of the Kubernetes CA's root certificate

### Using the admin key/certificate:
- Rest API Call: `curl https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt`
```json
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
```yaml
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

### ETCD SERVERS:
- ETCD can be deployed as a Cluster across multiple servers
- Additional Peer Certificates must be generated:
  - etcdpeer1.crt | etcdpeer1.key
  - etcdpeer2.crt | etcdpeer2.key
- Once created, specify them when starting the ETCD server (etcd.yaml)
- etcd.yaml:
```yaml
...
- etcd
  - --key-file=/path-to-certs/etcdserver.key
  - --cert-file=/path-to-certs/etcdserver.crt
  - --peer-cert-file=/path-to-certs/etcdpeer1.crt
  - --peer-client-cert-auth=true
  - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
  - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
  - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
  ...
```

### KUBE API SERVER:
- Most popular component, everyone talks to the Kube API Server:
- Various names:
  - KUBE-API SERVER
  - kubernetes
  - kubernetes.default
  - kubernetes.default.svc
  - kubernetes.default.svc.cluster.local
  - <host_ip_address>
- All these names MUST be present in the Kube API Server certificate
- Generate certificate with additional names:
  - `openssl genrsa -out apiserver.key 2048`
```
apiserver.key
```
  - Reference this file (openssl.cnf) with the next command:
```conf
[req]
req_extensions = v3_reg
distinguished_name = req_distinguised_name
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation,
subjectAltName = @alt_names
[alt_names]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
DNS.3 = kubernetes.default.svc
DNS.4 = kubernetes.default.svc.cluster.local
IP.1 = 10.96.0.1
IP.2 = 172.17.0.87
```
  - `openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr -config openssl.cnf`
```
apiserver.csr
```
  - `openssl x509 -req -in apiserver.csr -CA ca.crt -CAkey ca.key -out apiserver.crt`
```
apiserver.crt
```
- Configuration passed into the Kube API Servers executable or config file:
```conf
ExecStart=/usr/local/bin/kube-apiserver \\
  --etcd-cafile=/var/lib/kubernetes/ca.pem \\                                      # Connect to etcd
  --etcd-certfile=/var/lib/kubernetes/apiserver-etcd-client.crt \\                 # Connect to etcd
  --etcd-keyfile=/var/lib/kubernetes/apiserver-etcd-client.key \\                  # Connect to etcd
  --kubelet-certificate-authority=/var/lib/kubernetes/ca.pem \\                    # Connect to kubeletes
  --kubelet-client-certificate=/var/lib/kubernetes/apiserver-kubelet-client.crt \\ # Connect to kubeletes
  --kubelet-client-key=/var/lib/kubernetes/apiserver-kubelete-client.key \\        # Connect to kubeletes
  --client-ca-file=/var/lib/kubernetes/ca.pem \\                                   # CA cert
  --tls-cert-file=/var/lib/kubernetes/apiserver.crt \\                             # Kube API Server cert
  --tls-private-key-file=/var/lib/kubernetes/apiserver.key \\                      # Kube API Server key
  ...
```

### KUBELET NODES (SERVER CERT):
- Lives on each node in the cluster
- Named after the node it lives on
- They are specified on the kubelet-config.yaml:
```yaml
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
authentication:
  x509:
    clientCAFile: "/var/lib/kubernetes/ca.pem"
authorization:
  mode: Webhook
clusterDomain: "cluster.local"
clusterDNS:
  - "10.32.0.10"
podCIDR: "${POD_CIDR}"
resolveConf: "/run/systemd/resolve/resolv.conf"
runtimeRequestTimeout: "15m"
tlsCertFile: "/var/lib/kubelet/node01.crt"
tlsPrivateKeyFile: "/var/lib/kubelet/node01.key"
```

### KUBECTL NODES (CLIENT CERT)



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
