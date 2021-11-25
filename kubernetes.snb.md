# Explore the Kubernetes codebase with Sourcegraph

Explore Kubernetes repositories on GitHub. Search with examples below.

#### Use a ReplicationController configuration to ensure specified number of pod replicas are running at any one time

```sourcegraph
context:kubernetes file:pod.yaml content:"kind: ReplicationController"
```

#### Look for outdated `apiVersions` of admission webhooks

This apiVersion has been deprecated in favor of "admissionregistration.k8s.io/v1". You can read more about this at https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/

```sourcegraph
context:kubernetes content:"apiVersion: admissionregistration.k8s.io/v1beta1"
```

#### Find Prometheus usage in YAML files

```sourcegraph
context:kubernetes lang:yaml prom/prometheus
```

#### Search for examples of the sidecar pattern in Go

```sourcegraph
context:kubernetes lang:go sidecar
```

#### Browse diffs for recent code changes

```sourcegraph
context:kubernetes type:diff after:"1 week ago"
```
