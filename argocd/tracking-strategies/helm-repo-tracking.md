## Helm Repository Tracking Strategies

### Ways of Tracking Helm Repositories in Argo CD

- By using Specific Version
- By using Latest Version
- By using Version Range

### Example of Tracking Helm Repositories in Argo CD

1. **By using Specific Version:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    chart: mychart
    repoURL: https://bitnami-labs.github.io/sealed-secrets
    targetRevision: 1.16.0
```

2. **By using Latest Version:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    chart: mychart
    repoURL: https://bitnami-labs.github.io/sealed-secrets
    targetRevision: *
```

3. **By using Version Range:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    chart: mychart
    repoURL: https://bitnami-labs.github.io/sealed-secrets
    targetRevision: >=1.16.0 <1.17.0
```

Date of Notes: 27/04/2024