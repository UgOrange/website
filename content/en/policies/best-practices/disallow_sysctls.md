---
type: "docs"
title: Disallow Sysctls
linkTitle: Disallow Sysctls
weight: 19
description: >
    The Sysctl interface allows modifications to kernel parameters at runtime. In a Kubernetes pod these parameters can be specified under `securityContext.sysctls`. Kernel parameter modifications can be used for exploits and should be restricted.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/disallow_sysctls.yaml" target="-blank">/best-practices/disallow_sysctls.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-sysctls
  annotations:
    policies.kyverno.io/category: Security
    policies.kyverno.io/description: The Sysctl interface allows modifications to kernel parameters 
      at runtime. In a Kubernetes pod these parameters can be specified under `securityContext.sysctls`. 
      Kernel parameter modifications can be used for exploits and should be restricted.
spec:
  validationFailureAction: audit
  rules:
  - name: validate-sysctls
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Changes to kernel parameters are not allowed"
      pattern:
        spec:
          =(securityContext):
            X(sysctls): null
```