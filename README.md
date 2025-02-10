# Argo CD Example to Patch Existing DaemonSets

The following example patches existing DaemonSets running in a Nebius Managed Kubernetes (mk8s) cluster to add a custom toleration.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: tolerations-network-operator-ns
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/aaroniscode/argocd-patch-daemonsets
    path: network-operator
    targetRevision: HEAD
  syncPolicy:
    automated:
      prune: true
    syncOptions:
    - ServerSideApply=true
    - Validate=false
  destination:
    namespace: network-operator
    server: 'https://kubernetes.default.svc'
---
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: tolerations-kube-system-ns
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/aaroniscode/argocd-patch-daemonsets
    path: kube-system
    targetRevision: HEAD
  syncPolicy:
    automated:
      prune: true
    syncOptions:
    - ServerSideApply=true
    - Validate=false
  destination:
    namespace: kube-system
    server: 'https://kubernetes.default.svc'
```