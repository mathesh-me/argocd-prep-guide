## Argo CD Installation

Argo CD can be instaleed in two ways:

1. Multi-Tenant
2. Core

### Multi-Tenant Installation

The multi-tenant installation is the most common way to install Argo CD. This type of installation is typically **used to service multiple application developer teams in the organization and maintained by a platform team.**

There are two level of access we can have in Argo CD Installation:

1. **install.yaml**: Standard Argo CD installation with **cluster-admin access**. Use this manifest set if you plan to use Argo CD **to deploy applications in the same cluster** that Argo CD runs in (i.e. kubernetes.svc.default). It will still be able to deploy to external clusters with inputted credentials. For HA it will install multiple replicas for supported components. For Non-HA it will install single replica for supported components.
2. **namespace-install.yaml** - Installation of Argo CD which requires **only namespace level privileges** (does not need cluster roles). Use this manifest set if you do not need Argo CD to deploy applications in the same cluster that Argo CD runs in, and will **rely solely on inputted cluster credentials**. An example of using this set of manifests is if you run several Argo CD instances for different teams, where each instance will be deploying applications to external clusters. It will still be possible to deploy to the same cluster (kubernetes.svc.default) with inputted credentials (i.e. argocd cluster add <CONTEXT> --in-cluster --namespace <YOUR NAMESPACE>). For HA it will install multiple replicas for supported components. For Non-HA it will install single replica for supported components.

#### For Non-HA Installation

**install.yaml**
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

**namespace-install.yaml**
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/namespace-install.yaml
```

#### For HA Installation

**install.yaml**
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/ha/install.yaml
```

**namespace-install.yaml**
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/ha/namespace-install.yaml
```

### Core Installation

This type of installation is most suitable for **cluster administrators who independently use Argo CD** and **don't need multi-tenancy features**. This installation includes fewer components and is easier to setup. The bundle does not include the API server or UI, and installs the lightweight (non-HA) version of each component.

There are three ways to install Argo CD by using the Core Installation:

1. By usng Manifests:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml
```

2. By using Kustomize:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: argocd
resources:
- https://raw.githubusercontent.com/argoproj/argo-cd/v2.7.2/manifests/install.yaml
```

3. By using Helm:

The Helm chart is available at this [Location](https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd). You can install the Argo CD using the below command:

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm install my-release argo/argo-cd
```

Date of Notes: 28/04/2024