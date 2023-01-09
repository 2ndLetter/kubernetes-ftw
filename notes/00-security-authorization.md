# Authorization:
## Documentation:
- https://kubernetes.io/docs/reference/access-authn-authz/authorization/

## Notes/Commands:
- Authorization Mechanisms:
  - Node:
  - ABAC:
  - RBAC:
  - Webhook:

- Node Authorizer:
  - A special-purpose authorization mode that grants permissions to kubelets based on the pods they are scheduled to run. To learn more about using the Node authorization mode
  - Part of the SYSTEM:NODES group
  - sysem:node:node01, using the kubelet, is authorized by the Node Authorizer
- ABAC:
  - Attribute-based access control (ABAC) defines an access control paradigm whereby access rights are granted to users through the use of policies which combine attributes together. The policies can use any type of attributes (user attributes, resource attributes, object, environment attributes, etc).
  - Associate user or group o fusers
- RBAC:
  - Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within an enterprise. In this context, access is the ability of an individual user to perform a specific task, such as view, create, or modify a file.
- Webhook:
  - A WebHook is an HTTP callback: an HTTP POST that occurs when something happens; a simple event-notification via HTTP POST. A web application implementing WebHooks will POST a message to a URL when certain things happen.
