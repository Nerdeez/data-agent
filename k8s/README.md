# Cluster manifests (GitOps)

Kubernetes workload configuration for the [data-agent](https://github.com/Nerdeez/data-agent) workshop. **Argo CD** syncs everything under [`apps/`](apps/) from this repository.

Infrastructure (GKE, VPC, Argo CD install) stays in [`iac/live/`](../iac/live/).

## Layout

| Path | Purpose |
|------|---------|
| [`apps/`](apps/) | Desired cluster state — add Namespaces, Deployments, Services, etc. here (Kustomize). |
| [`argocd/application.yaml`](argocd/application.yaml) | Argo CD `Application` CR — **bootstrap only** (not part of the synced path). |

## Bootstrap Argo CD (run once)

After Argo CD is installed (Terragrunt `iac/live/argocd`) and `kubectl` points at the cluster:

```bash
kubectl apply -f k8s/argocd/application.yaml
```

Or with the CLI (same manifest):

```bash
argocd login <ARGOCD_SERVER> --username admin --password <...> --insecure
kubectl apply -f k8s/argocd/application.yaml
```

Check sync:

```bash
argocd app get data-agent
argocd app sync data-agent   # if not using automated sync yet
kubectl get all -n data-agent
```

## Day‑2 workflow

1. Change manifests under `k8s/apps/`.
2. Commit and push to `main` on GitHub.
3. Argo CD reconciles automatically (`syncPolicy.automated` in the Application).

## Private repository

If the repo is private, add credentials in Argo CD before the Application can sync:

```bash
argocd repo add https://github.com/Nerdeez/data-agent.git \
  --username git --password <GITHUB_PAT>
```

Or configure a GitHub App / deploy key in the Argo CD UI (**Settings → Repositories**).

## Adding DataAgent

Place workload manifests (or a Kustomize overlay) under `k8s/apps/` and list them in [`apps/kustomization.yaml`](apps/kustomization.yaml).
