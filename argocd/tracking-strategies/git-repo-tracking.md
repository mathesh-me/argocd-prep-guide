## Git Repo Tracking

### Ways of Tracking Git Repositories in Argo CD

- By using Commit SHA
- By using Tag
- By using Branch
- By using HEAD

### Example of Tracking Git Repositories in Argo CD

1. **By using Commit SHA:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    path: guestbook
    targetRevision: 3f7a1f3
```

2. **By using Tags:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    path: guestbook
    targetRevision: v1
```

3. **By using Branch:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: main
    path: guestbook
```

4. **By using HEAD:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD # It is like a Symbolic Link to the latest commit
    path: guestbook
```

Date of Notes: 27/04/2024