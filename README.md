# argocd
ArgoCD with all related things

Initial app of apps configuration:
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-of-apps
  namespace: argocd
spec:
  destination:
    namespace: argocd
    server: https://kubernetes.default.svc
  project: default
  source:
    directory:
      jsonnet: {}
      recurse: true
    path: resources/v-01/app-of-apps
    repoURL: https://github.com/janpastrnak/argocd
    targetRevision: main
  syncPolicy:
    automated:
      selfHeal: true
    syncOptions:
    - Validate=false
    - CreateNamespace=true
```