
`kubectl expose deployment my-deployment --type=NodePort`
`kubectl port-forward my-pod-asdfasf 5000:80`
`kubectl attach my-pod-asdfasdf -c mycontainer`
`kubectl exec -it my-pod-asdfas /bin/sh`
`kubectl label pods my-pod-asdf [--overwrite] alabel=nope`
`kubectl run echo 'hi' --image=busybox --port=5701`
`kubectl rollout status`
`kubectl set image`
`kubectl rollout history`
`kubectl proxy`


https://kubernetes-security.info/#authenticationKubernetes Master(s)
* API Server
* Scheduler
* Controller Manager
* ETCD(DB?)

Kubernetes Nodes
* Kubelet (Bare)
  Runs pods on a node
* kube-proxy (Bare)
  Ensures that access is configured as the deployments dictate
* Docker
* AnyPod


# Health Checks

## Readiness Probe
Determine when a pod has loaded and has what it needs internally and is ready for external requests

## Liveness Probes
Determines whether a pod is healthy or unhealthy after readiness has been determined

## DNS names
All non-headless services get a name
`<my-service>.<my-namespace>.svc.cluster.local`

Find internal nameserver
`cat /etc/resolve.conf`

# Resource monitoring
## Heapster
Pod installed alongside each .
CNCF project
`Heapster -> Kubelet -> CAdvisor`

## InfluxDB
Stores data sent from Heapster

## Grafana
Used to chart data from InfluxDB

# Service Accounts

The identity token exists at `/var/run/secrets/kubernetes.io/serviceaccount/token`
Attach a serviceaccount by name in `Pod.spec.serviceAccountName`


`system:serviceaccount:<NAMESPACE>:<NAME>`

Applications are given a default service account.

# K8s auth flow

Account Impersonation https://kubernetes.io/docs/reference/access-authn-authz/authentication/#user-impersonation

* Client (User/App) sends creds to API server
* API server uses one of k8s configured authentication plugins
* Identity provider confirms group membership and identity
* API server verifies permissions

# Security

K8s pen testing apps
https://github.com/aquasecurity/kube-hunter
https://github.com/aquasecurity/kube-bench

Use CIS benchmark for latest reccomendations for k8s and docker

https://www.cisecurity.org/benchmark/kubernetes/
https://www.cisecurity.org/benchmark/docker/

Ensure 8080 is closed (Insecure port). Validate by `curl`ing the clusters IP

Set anonymous-auth to false. Default RBAC only allows discovery

Enable `RBAC` and `Node` with --authorization-mode

## Kubelet

* Disable anonymous access
* Ensure that requests are authorized authorization-mode
* Limit the permissions of kubelets `admission-control=NodeRestriction`
* --read-only-port=0
* Older k8s versions should disable cAdvisor port `--cadvisor-port=0`
* Enable cert rotation `--rotate-certificates`

Kubelet interacts with container runtimes to start/stop pods. 

Each kubelet exposes an internal API. If unauthorized users can access the API they can gain control of the cluster.

`curl -sk https://<IP address>:10250/pods/`

## ETCD

It's critical there is no unauthorized access to ETCD as it is the KV store for kubernetes

* Set `--cert-file` and `--key-file` to enable HTTPS to etcd
* `--client-cert-auth=true` `--trusted-ca-file`
* Set `--auto-tls=false` to disable automatic certificate generation
* `--peer-client-cert-auth=true` and  `--peer-auto-tls=false` `--peer-cert-file --peer-key-file` `--peer-trusted-ca-file` To ensure TLS across ETCD nodes
* Set `--etcd-cafile` to the CA the 
* Set --etcd-certfile --etcd-keyfile so the API server can identify itself to the ETCD server
* https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/security.md
* Encrypt ETCD's data stored on disk https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

## K8s Dash

* Prevent unauthed access
* Clicking skip allows the user access at the level of the dashboards service account
* Block access publicly
* Use `kubectl proxy` to access privately

# RBAC

Generate RBAC from audit logs
https://github.com/liggitt/audit2rbac

ClusterWide|NamespaceWide|

* Entity: Group|User|Service Account
* Resource: Pod, service, or secret
* Role: Defines rules for actions on resources
* Role Binding: Binds a role to an entity
* Verbs: ( get, list ) (create, update, patch, delete, deletecollection)

K8s prevents privilege escalation. A user can only grant access to permissions that it has access to itself.

Built in roles https://kubernetes.io/docs/reference/access-authn-authz/rbac/#default-roles-and-role-bindings

Set `Pod.spec.automountServiceAccountToken` to false to prevent ALL access

# Containers

Scan them lol

Prevent non-signed images https://github.com/IBM/portieris

Pull containers by digest, not tag
OR use AlwaysPullImages

# Admission Controllers

After authentication, prevent resources from being submitted that would violate one of these controls

* AlwaysPullImages
  Not enabling this controller allows any pod running on the node to access the docker cache and access that image.  This forces an authentication check to the registry to ensure the pod has the correct authority to access the image
* DenyEscalatingExec
  Prevent users from attaching via shell or exec to pods that run with privs above the current user.
* PodSecurityPolicy ?
* LimitRange and ResourceQuota
  Helps prevent DoS attacks by preventing too many requests or too many resources being required
* NodeRestriction
  Limit nodes kubelet permissions

# Security Boundaries

1. Cluster
1. Node
1. Namespace
1. Pod
1. Container

![Isolation Boundaries](@attachment/isolation-provided-by-layer-of-Kubernetessyjn.png)

# Security Contexts

Allow the pod to only be run as a specific user
`Pod.spec.securityContext.runAsUser = 1001`

Prevent privilege escalation
`Pod.spec.containers.securityContext.allowPrivilegeEscalation = false`

Disable writing files within the container if the application doesn't need it.

enable 
`allowedHostPaths`
Disable
```
allowPrivilegeEscalation
privileged
```
# Network Security

Not all k8s networking solutions support network policies. Consider calico, openshift CDN, Cilium, weave-net

network policy applies to the set of pods that match the podSelector

https://kubernetes.io/docs/concepts/services-networking/network-policies/#default-policies
By default pods can communicate with external hosts (North-South) as well as internal containers (East-West)

