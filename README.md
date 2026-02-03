# Kubernetes GitOps with FluxCD

This repository demonstrates how to set up a GitOps workflow using Kubernetes and FluxCD.

## Prerequisites

- A Kubernetes cluster

- `kubectl` installed and configured

- [FluxCD CLI](https://fluxcd.io/flux/installation/) installed

- A GitHub account with a personal access token

## Steps to Set Up


### Step 1: Apply Kubernetes Manifests

Apply the necessary Kubernetes manifests to create the namespace, deployment, and service:


```bash
cd manifests/
kubectl apply -f namespace.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### Step 2: Install FluxCD

Follow the [FluxCD installation guide](https://fluxcd.io/flux/installation/) to install the FluxCD CLI.

### Step 3: Create a GitHub Personal Access Token

Generate a new token with the required permissions for repository access.

1. Go to GitHub account settings.

2. Navigate to Developer Settings > Personal Access Tokens > Tokens(classic) > Generate New Token.


### Step 4: Export the GitHub Token

Export the GitHub token as an environment variable

```bash
export GITHUB_TOKEN= <github-token>
```

### Step 5: Bootstrap FluxCD

Bootstrap FluxCD to GitHub repository:

```bash
flux bootstrap github \
  --token-auth \
  --owner=<github-username> \
  --repository=<github-repository-name> \
  --branch=main \
  --path=clusters/<Kubernetes-cluster-name> \
  --personal
```

### Step 6: Sync Repository Changes

Ensure the local repository is up-to-date:

```bash
git fetch
git pull 
```

### Step 7: Reconcile FluxCD

Reconcile the FluxCD Git source to apply the latest changes:

```bash
flux reconcile source git -n flux-system flux-system 
```

## Notes

* Ensure your Kubernetes cluster has sufficient resources to deploy the application.

* Replace placeholders (`<github-username>`, `<github-repository-name>`, `<Kubernetes-cluster-name>`, `<github-token>`) with actual values.
