# Certificates API:
## Documentation:
- https://kubernetes.io/docs/home/
---

## Commands/Notes:
- Certificate Signing Process:
  - Create CertificateSigningRequest Object
  - Review Requests
  - Approve Requests
  - Share Certs to Users
- Full Process:
  - User generates a private key and certificate signing request:
  - `openssl genrsa -out jane.key 2048`
  ```
  jane.key
  ```
  - `openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr`

  ```
  jane.csr
  
  -----BEGIN CERTIFICATE REQUEST-----
  ...
  ...
  -----END CERTIFICATE REQUEST-----
  ```
  - User send the jane.csr to the Admin
  - Admin runs this command to retrieve the "request" data: `cat jane.csr | base64 | tr -d "\n"`
  - The Admin creates a CertificateSigningRequest Object (jane-csr.yaml):
  ```yaml
  apiVersion: certificates.k8s.io/v1beta1
  kind: CertificateSigningRequest
  metadata:
    name: jane
  spec:
    groups:
    - system:authenticated
    usages:
    - digital signature
    - key encipherment
    - server auth
    request:
      LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0
      tLS0KTUlJQ1dEQ0NBVUFDQVFBd0V6RVJNQThHQTFVRU
      F3d0libVYzTFhWelpYSXdnZ0VpTUEwR0NTcUdTSWIzR
      FFFQgpBUVVBQTRJQkR3QXdnZ0VLQW9JQkFRRE8wV0pX
      K0RYc0FKU0lyanBObzV2UklCcGxuemcrNnhjOStVVnd
      rS2kwCkxmQzI3dCsxZUVuT041TXVxOTlOZXZtTUVPbn
  ```

## KodeKloud Lab:
- `cat /etc/kubernetes/manifests/kube-apiserver.yaml`
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
