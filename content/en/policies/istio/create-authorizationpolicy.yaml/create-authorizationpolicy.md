---
title: "Create Istio Deny AuthorizationPolicy"
category: Istio
version: 1.6.0
subject: AuthorizationPolicy
policyType: "generate"
description: >
    An AuthorizationPolicy enables access controls on workloads in the mesh. It supports per-Namespace controls which can be a union of different behaviors. This policy creates a default deny AuthorizationPolicy for all new Namespaces. Further AuthorizationPolicies should be created to more granularly allow traffic as permitted. Use of this policy will likely require granting the Kyverno ServiceAccount additional privileges required to generate AuthorizationPolicy resources.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//istio/create-authorizationpolicy.yaml/create-authorizationpolicy.yaml" target="-blank">/istio/create-authorizationpolicy.yaml/create-authorizationpolicy.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: create-authorizationpolicy
  annotations:
    policies.kyverno.io/title: Create Istio Deny AuthorizationPolicy
    policies.kyverno.io/category: Istio
    policies.kyverno.io/severity: medium
    kyverno.io/kyverno-version: 1.8.0
    policies.kyverno.io/minversion: 1.6.0
    kyverno.io/kubernetes-version: "1.24"
    policies.kyverno.io/subject: AuthorizationPolicy
    policies.kyverno.io/description: >-
      An AuthorizationPolicy enables access controls on workloads in the mesh. It supports per-Namespace controls
      which can be a union of different behaviors. This policy creates a default deny AuthorizationPolicy
      for all new Namespaces. Further AuthorizationPolicies should be created to more granularly allow
      traffic as permitted. Use of this policy will likely require granting the Kyverno ServiceAccount
      additional privileges required to generate AuthorizationPolicy resources.
spec:
  rules:
  - name: generate-deny-authorizationpolicy
    match:
      any:
      - resources:
          kinds:
          - Namespace
    generate:
      apiVersion: security.istio.io/v1beta1
      kind: AuthorizationPolicy
      name: default-deny
      namespace: "{{request.object.metadata.name}}"
      synchronize: true
      data:
        spec: {}
```
