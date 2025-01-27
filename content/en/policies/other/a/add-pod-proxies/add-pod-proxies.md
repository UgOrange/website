---
title: "Add Pod Proxies"
category: Sample
version: 1.6.0
subject: Pod
policyType: "mutate"
description: >
    In restricted environments, Pods may not be allowed to egress directly to all destinations and some overrides to specific addresses may need to go through a corporate proxy. This policy adds proxy information to Pods in the form of environment variables. It will add the `env` array if not present. If any Pods have any of these env vars, they will be overwritten with the value(s) in this policy.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/a/add-pod-proxies/add-pod-proxies.yaml" target="-blank">/other/a/add-pod-proxies/add-pod-proxies.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: add-pod-proxies
  annotations:
    policies.kyverno.io/title: Add Pod Proxies
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/category: Sample
    policies.kyverno.io/minversion: 1.6.0
    policies.kyverno.io/description: >-
      In restricted environments, Pods may not be allowed to egress directly to all destinations
      and some overrides to specific addresses may need to go through a corporate proxy.
      This policy adds proxy information to Pods in the form of environment variables.
      It will add the `env` array if not present. If any Pods have any of these
      env vars, they will be overwritten with the value(s) in this policy.
spec:
  rules:
  - name: add-pod-proxies
    match:
      any:
      - resources:
          kinds:
          - Pod
    mutate:
      patchStrategicMerge:
        spec:
          containers:
            - (name): "*"
              env:
              - name: HTTP_PROXY
                value: http://proxy.corp.domain:8080
              - name: HTTPS_PROXY
                value: https://secureproxy.corp.domain:8080
              - name: NO_PROXY
                value: localhost,*.example.com
```
