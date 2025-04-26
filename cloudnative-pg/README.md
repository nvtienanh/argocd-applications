# CloudNativePG Operator Argo CD

```bash
kubectl apply -f argocd.yaml
```

# Create a Postgre databse

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: sample-db
  namespace: argocd
  finalizers:
    - resources-finalizer.argocd.argoproj.io
  labels:
    name: sample-db
spec:
  syncPolicy:
    automated:
      selfHeal: true
      prune: true
      allowEmpty: false
    syncOptions:
      - Validate=false
      - CreateNamespace=true
      - PrunePropagationPolicy=foreground
      - PruneLast=true
      - RespectIgnoreDifferences=true
      - Replace=true
  project: default
  source:
    chart: cluster
    repoURL: https://cloudnative-pg.github.io/charts
    targetRevision: 0.*
    helm:
      releaseName: cnpg
      passCredentials: false
      parameters:
        - name: "fullnameOverride"
          value: "pgsql"
        - name: "cluster.instances"
          value: "1"
        - name: "cluster.storage.storageClass"
          value: "nfs-csi"
        - name: "cluster.walStorage.enabled"
          value: "true"
        - name: "cluster.walStorage.storageClass"
          value: "nfs-csi"
        - name: "cluster.monitoring.enabled"
          value: "true"
  destination:
    server: "https://kubernetes.default.svc"
    namespace: mydb
  revisionHistoryLimit: 3
```