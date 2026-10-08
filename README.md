# data-agent-argo

This repository deploys **DataAgent** onto a Kubernetes cluster that is managed with **Argo CD**. It holds the configuration and automation needed to install and operate DataAgent in that GitOps-driven environment.

## Prerequisites: [mise](https://mise.jdx.dev/)

Tool versions for this project are pinned in [`mise.toml`](mise.toml). [mise](https://mise.jdx.dev/) installs those binaries and puts them on your `PATH` when you are in this repository.

This project uses Terragrunt, OpenTofu, kubectl, and the Argo CD CLI. Exact versions are defined in [`mise.toml`](mise.toml).

### Install mise

Pick one method that matches your setup:

**macOS (Homebrew)**

```bash
brew install mise
```

**Shell installer (Linux, macOS, WSL)**

```bash
curl https://mise.run | sh
```

Then enable mise in your shell. For example, with zsh, add this to `~/.zshrc`:

```bash
eval "$(mise activate zsh)"
```

Restart your shell or run `source ~/.zshrc`. Other shells are documented in the [mise getting started guide](https://mise.jdx.dev/getting-started.html).

### Install project tools

From the root of this repository:

```bash
mise trust    # only if mise asks you to trust this config
mise install
```

`mise install` reads `mise.toml` and installs those tools at the pinned versions. Verify with:

```bash
terragrunt --version
tofu --version
kubectl version --client
argocd version --client
```

## GitOps (`k8s/`)

Cluster workload manifests live in [`k8s/apps/`](k8s/apps/). Register the Argo CD `Application` once (see [`k8s/README.md`](k8s/README.md)), then push changes to [github.com/Nerdeez/data-agent](https://github.com/Nerdeez/data-agent) for Argo CD to reconcile.
