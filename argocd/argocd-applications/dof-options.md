## Directory of Files (DOF) Options

Argo CD provides below options for **Directory of Files (DOF)**:

- **Recursive**: If set to true, Argo CD will recursively search for all manifests in the **Sub directories also**.
- **Jsonnet**: 
    - **ExtVars**: We can pass external variables to the Jsonnet files.
    - **TLA vars**: We can pass Top-level arguments to the Jsonnet files.

### Example

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD
    path: directories-of-manifests
    directory:
      recursive: true
      jsonnet:
        extVars:
          - name: replicas
            value: 3
        tlas:
          - name: replicas
            value: 3
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

Date of Creation: 27/04/2024