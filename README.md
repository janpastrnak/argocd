# argocd
ArgoCD with all related things

Initial app of apps configuration:

```apiVersion: argoproj.io/v1alpha1
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

## Suggested git dir structures

### App based

```argocd/
├── base
│   └── app-X
│       ├── deployment.yaml
│       ├── kustomization.yaml
│       └── service.yaml
        new-app
└── overlays
    ├── cc-test
    |   └── app-X  # ref to ../../base/app-X
    ├── cc-test-<cluster>
        └── app-X  # ref to ../cc-test/app-X
            new-app
        cc-test-cluster-1
            new-app
```

### Server based

```kustomize-example-app/
├── base
│   ├── deployment.yaml
│   ├── kustomization.yaml
│   └── service.yaml
└── overlays
    ├── int cc-test
    │   ├── deployment.yaml
    │   ├── kustomization.yaml
    │   ├── namespace.yaml
    │   └── service.yaml
    ├── prod # cc-prod
    │   ├── deployment.yaml
    │   ├── kustomization.yaml
    │   ├── namespace.yaml
    │   └── service.yaml
    └── test  # cc-test
        ├── deployment.yaml
        ├── kustomization.yaml # ref to ../../../base
        ├── namespace.yaml
        └── service.yaml
        cc-test-cluster-1
            kustomization.yaml  # ref to ../test/
```
