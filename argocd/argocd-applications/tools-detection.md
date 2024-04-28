## Tools(Format) Detection in Argo CD

By Default, Argo CD uses the following formats to **Detect the tools**:

1. **Kustomize**: If the application source is a Git repository and the path contains a `kustomization.yaml` file, Argo CD will automatically detect the format as Kustomize.
2. **Helm**: If the application source is a Git repository and the path contains a `Chart.yaml` file, Argo CD will automatically detect the format as Helm.
3. **Ksonnet**: If the application source is a Git repository and the path contains a `app.yaml` file, Argo CD will automatically detect the format as Ksonnet.
4. **Directory**: If the application source is a Git repository and the path contains a directory with Kubernetes manifests, Argo CD will automatically detect the format as Directory.

#### How to Explicitly Set the tools Format?

We can explicitly set the tools format in the application definition YAML file. We can use the `source.ksonnet`, `source.helm`, `source.kustomize`, and `source.directory` fields to set the tools format.

1. Example for `Kustomize`:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: guestbook
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD
    kustomize:
      namePrefix: guestbook-
      commonLabels:
        app.kubernetes.io/name: guestbook
        app.kubernetes.io/instance: guestbook
        app.kubernetes.io/managed-by: argocd
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

2. Example for `Helm`:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: guestbook
    repoURL:
    targetRevision: HEAD
    helm:
      valueFiles:
        - values.yaml
      chart: guestbook
      version: 1.0.0
      repoURL: https://argoproj.github.io/argocd-example-apps
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

3. Example for `Ksonnet`:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: guestbook
    repoURL:
    targetRevision: HEAD
    ksonnet:
      environment: default
      component: guestbook
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

4. Example for `Directory`:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: guestbook
    repoURL:
    targetRevision: HEAD
    directory:
      recurse: true
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

In the above examples, we have explicitly set the tools format in the application definition YAML file. Argo CD will use the specified format to deploy the application.<br>

We can also Explicitly set the tools format using the **Argo CD Web UI** also. We can select the format from the dropdown while creating the application.

Date of Creation: 26/04/2024