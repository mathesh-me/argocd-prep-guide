## Kustomize Options in ArgoCD

Argo CD provide below options for **Kustomize**:

- **Name prefix:** Will get prepended to all resources.
- **Name suffix:** Will get appended to all resources.
- **Images:** We can set the images for all resources.
- **Common labels:** Set labels on all resources.
- **Common annotations:** We can set annotations on all resources.
- **Version:** To explicitly set kustomize version.

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
    path: guestbook
    kustomize:
      namePrefix: dev-
      nameSuffix: -dev
      images:
        - gcr.io/heptio-images/ks-guestbook-demo:0.2
      commonLabels:
        app.kubernetes.io/managed-by: argocd
      commonAnnotations:
        note: This is a guestbook application
      version: v3.8.1
  destination:
    server: https://kubernetes.default.svc
    namespace: default 
  project: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

Date of Creation: 27/04/2024