# Helm Chart Repository

## Available Charts

### project-a

#### DEV Environment
```bash
helm repo add project-a-dev https://aja2k.github.io/pp/helm-repo/project-a/dev
```

Available Charts:
```
- argo-rollouts (version: 2.37.7)
  A Helm chart for argo-rollouts
- argocd (version: 1.0.1)
  A Helm chart for argoCD
- calico-dev (version: 1.0.0)
  A Helm chart for calico-CNI
- ingress-nginx-dev (version: 1.0.1)
  A Helm chart for ingress-nginx
- metallb-crd-dev (version: 1.0.0)
  A Helm chart for metallb-crd
- metallb-dev (version: 1.0.1)
  A Helm chart for metallb
- metrics-server-dev (version: 1.0.1)
  A Helm chart for metrics-server
- nfs-k8s (version: 1.0.0)
  A Helm chart for NFS (DEV)
- prometheus (version: 26.0.0)
  A Helm chart for prometheus (DEV)
```

#### STG Environment
```bash
helm repo add project-a-stg https://aja2k.github.io/pp/helm-repo/project-a/stg
```

Available Charts:
```
- prometheus (version: 26.0.0)
  A Helm chart for prometheus (STG)
- stg-argo-rollouts (version: 2.37.7)
  A Helm chart for argo-rollouts
- stg-ingress-nginx (version: 4.11.2)
  A Helm chart for ingress-nginx
- stg-nfs-subdir (version: 4.0.18)
  A Helm chart for Kubernetes Cluster (STG)
```

#### PRD Environment
```bash
helm repo add project-a-prd https://aja2k.github.io/pp/helm-repo/project-a/prd
```

Available Charts:
```
- prd-argo-rollouts (version: 2.37.7)
  A Helm chart for argo-rollouts
- prd-ingress-nginx-ext (version: 4.11.2)
  A Helm chart for ingress-nginx
- prd-nfs-subdir (version: 4.0.18)
  A Helm chart for Kubernetes Cluster (PRD)
- prometheus (version: 26.0.0)
  A Helm chart for prometheus (PRD)
```

