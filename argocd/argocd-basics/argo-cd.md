## Argo CD

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. It follows the GitOps pattern of using Git repositories as the source of truth for defining the desired application state. Argo CD automates the deployment of the desired application states in the specified target environments.

### How Argo CD Works in a Nutshell?

**Argo CD will continuously monitor the desired application state defined in the Git repository and compare it with the live state of the application running on the Kubernetes cluster**. If there is any drift between the desired state and the live state, Argo CD will automatically reconcile the drift.


## Argo CD Architecture

![Argo CD Architecture](https://raw.githubusercontent.com/argoproj/argo-cd/stable/docs/assets/argocd_architecture.png)

### Components

### API Server
The API server is a gRPC/REST server which exposes the API consumed by the `Web UI, CLI, and CI/CD`. When we interact with Argo CD, we are  actually interacting with the API server.

**Dex** is an OpenID Connect Identity (OIDC) provider used for authentication. If we want to use OIDC for authentication, we can configure Dex with Argo CD.

### Repository Server
**The repository server will be connect to the Git repository and fetch the manifests**. It will maintain a local cache of the Git repository and watch for changes in the repository.

### Application Controller
**The application controller is responsible for the reconciliation of the desired application state defined in the Git repository with the live state of the application running on the Kubernetes cluster**. It will continuously monitor the desired application state defined in the repo server and compare it with the live state of the application running on the Kubernetes cluster. If there is any drift between the desired state and the live state, the application controller will automatically reconcile the drift.

### Application Set Controller
Automating the generation of Argo CD Applications.

### Redis
Argo CD uses Redis as a caching layer to store the application state.

### Installation

We can Install Argo CD using three methods:

1. **kubectl**: We can install Argo CD using kubectl. This method is useful for testing and development purposes.
2. **Helm**: We can install Argo CD using Helm. This method is useful for production purposes.
3. **Operator**: We can install Argo CD using the Argo CD Operator. This method is useful for production purposes.

### Not Opiniated

Argo CD is **not opinionated** about the way you structure your application manifests. You can use any templating tool like Helm, Kustomize, Ksonnet, or Kustomize to generate the manifests.

Date of Notes: 16/04/2024