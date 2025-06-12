## Helm

### Declarative

You can install Helm charts through the UI, or in the declarative GitOps way.  
Helm is only used to inflate charts with `helm template`. The lifecycle of the application is handled by Argo CD instead of Helm.
Here is an example:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: kube-prometheus-stack
  namespace: argocd
spec:
  destination:
    namespace: observability
    server: https://kubernetes.default.svc
  project: tbs-dev
  source:
    helm:
      releaseName: deka-metric
      valueFiles:
      - values.yaml
    path: charts/kube-prometheus-stack
    repoURL: git@github.com:Lintasarta/prometheus-community.git
    targetRevision: main
  syncPolicy:
    syncOptions:
    - CreateNamespace=true

```

!!! note "When using Helm there are multiple ways to provide values"
    Order of precedence is `parameters > valuesObject > values > valueFiles > helm repository values.yaml`.