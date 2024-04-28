## Helm Options

### Argo CD Sources for Helm

- **Git Repository**
- **Helm Repository**

### Helm Options

We can define our source as `Helm Repository` in the Argo CD Application. We can use the below YAML file to define an application in Argo CD using Helm Repository:

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
    chart: helm-guestbook
    targetRevision: 1.61.0
    repoURL: https://bitnami-labs.github.io/sealed-secrets
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

Example with `Helm Application in Git Repository`:

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
    path: helm-guestbook
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

**Argo CD Provide below Options for Helm Applications:**

- **Helm - Values files**: We can specify the values file for the Helm Application.
- **Helm - Parameters**: We can override the values in `values.yaml` using the Helm Parameters.
- **Helm - File Parameters**: Set Parameters values form Files
- **Helm - Values as Block**: Set the values as a block in the Application YAML file.

**Argo CD Helm Options Example:**

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
    path: helm-guestbook
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD
    helm:
      valueFiles:
        - values-demo.yaml
      parameters:
        - name: replicaCount
          value: "2"
      fileParameters:
        - name: config
          path: files/config.json
      values: |
        replicaCount: 2
        image:
          repository: nginx
          tag: stable
          pullPolicy: IfNotPresent
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

Date of Creation: 26/04/2024