Explore Kubernetes repositories on GitHub. Search with examples below.

# Search examples

#### Use a ReplicationController configuration to ensure specified number of pod replicas are running at any one time

```sourcegraph
repogroup:kubernetes file:pod.yaml content:"kind: ReplicationController"
```

#### Look for outdated `apiVersions` of admission webhooks
This apiVersion has been deprecated in favor of "admissionregistration.k8s.io/v1". You can read more about this at https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/

```sourcegraph
repogroup:kubernetes content:"apiVersion: admissionregistration.k8s.io/v1beta1"
```

#### Find Prometheus usage in YAML files

```sourcegraph
repogroup:kubernetes lang:yaml prom/prometheus
```

#### Search for examples of the sidecar pattern in Go

```sourcegraph
repogroup:kubernetes lang:go sidecar
```

#### Browse diffs for recent code changes

```sourcegraph
repogroup:kubernetes type:diff after:"1 week ago"
```
