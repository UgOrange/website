---
title: "Restrict AppArmor"
category: Pod Security Standards (Baseline)
policyType: "validate"
description: >
    On supported hosts, the 'runtime/default' AppArmor profile is applied by default.  The default policy should prevent overriding or disabling the policy, or restrict  overrides to an allowed set of profiles.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/baseline/restrict-apparmor-profiles/restrict-apparmor-profiles.yaml" target="-blank">/pod-security/baseline/restrict-apparmor-profiles/restrict-apparmor-profiles.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-apparmor-profiles
  annotations:
    policies.kyverno.io/title: Restrict AppArmor
    policies.kyverno.io/category: Pod Security Standards (Baseline)
    policies.kyverno.io/severity: medium
    policies.kyverno.io/minversion: 1.3.0
    policies.kyverno.io/description: >-
      On supported hosts, the 'runtime/default' AppArmor profile is applied by default. 
      The default policy should prevent overriding or disabling the policy, or restrict 
      overrides to an allowed set of profiles.
spec:
  validationFailureAction: audit
  background: true
  rules:
    - name: app-armor
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: >-
          Specifying other AppArmor profiles is disallowed. The annotation
          container.apparmor.security.beta.kubernetes.io must not be defined,
          or must not be set to anything other than `runtime/default`.
        pattern:
          metadata:
            =(annotations):
              =(container.apparmor.security.beta.kubernetes.io/*): "runtime/default"

```