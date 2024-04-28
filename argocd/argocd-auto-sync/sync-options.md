## Sync Options

We can set the sync options for the Argo CD applications. The sync options are used **to control the behavior of the sync process**. We have Sync options at application level and also at resource level.

Below are the Sync options available in Argo CD:

- **No Prune**: Resource Level 
- **Disable Validation**: Application Level, Resource Level
- **Selective Sync**: Application Level
- **Prune Last**: Application Level, Resource Level
- **Replace Resource On Sync**: Application Level, Resource Level
- **Fail on Shared Resource**: Application Level
- **Create Namespace**: Application Level

---

## How to Set Sync Options?

We can set the sync options in the application manifest file or using the Argo CD UI or Argo CD CLI.

### Declarative Way:

- For Resource Level Sync Options, we can set the sync options in the resource manifest file by using annotations `argocd.argoproj.io/sync-options`.
- For Application Level Sync Options, we can set the sync options in the application manifest file by using the `syncPolicy.syncOptions` field.

Below is the example of setting the sync options in the application manifest file at Application Level:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL:
    targetRevision: HEAD
    path: my-app 
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated: {}
    syncOptions:
      - <sync-option>=<value>
```

Below is the example of setting the sync options in the resource manifest file at Resource Level:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
  namespace: default
  annotations:
    argocd.argoproj.io/sync-options: <sync-option>=<value>
data:
  key: value
```


### No Prune

If we want to prevent the de;etion of Objects from the cluster, We can use this `No Prune` option. Our application will be in Out of Sync state and will expect us to delete the objects.

**At Resource Level:**

```yaml
metadata:
  annotations:
    argocd.argoproj.io/sync-options: Prune=false
```

### Disable Validation

For some specific objects we may not want validation. So in that case We can disable the validation of the resources using the `DisableValidation` sync option.

**At Resource Level:**

```yaml
metadata:
  annotations:
    argocd.argoproj.io/sync-options: Validate=false
```

**At Application Level:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  syncPolicy:
    syncOptions:
    - Validate=false
```

### Selective Sync

We can set the `syncOptions` to `ApplyOutOfSyncOnly` in the application manifest file to sync only the out-of-sync resources. By default, Argo CD syncs all the resources whether they are in sync or out of sync.

**At Application Level:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  syncPolicy:
    syncOptions:
    - ApplyOutOfSyncOnly=true
```

### Prune Last

We can set the `syncOptions` to `PruneLast` in the application manifest file to delete the resources after the new resources are created. This option is useful when we want to delete the resources after the new resources are created.

**At Application Level:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  syncPolicy:
    syncOptions:
    - PruneLast=true
```

**At Resource Level:**

```yaml
metadata:
  annotations:
    argocd.argoproj.io/sync-options: PruneLast=true
```

### Replace Resource On Sync

Normally Argo CD uses the `kubectl apply` command to sync the resources. If we want to use the `kubectl replace` command instead of `kubectl apply` command, we can set the `syncOptions` to `Replace=true`. Example: resource spec might be too big and won't fit into kubectl.kubernetes.io/last-applied-configuration annotation that is added by kubectl apply. In such cases we might use Replace=true sync option

**At Application Level:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  syncPolicy:
    syncOptions:
    - Replace=true
```

**At Resource Level:**

```yaml
metadata:
  annotations:
    argocd.argoproj.io/sync-options: Replace=true
```

### Fail on Shared Resource

If we want to fail the sync process when a resource is shared between multiple applications, we can set the `syncOptions` to `FailOnSharedResource=true`. By default, Argo CD does not fail the sync process when a resource is shared between multiple applications.

**At Application Level:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  syncPolicy:
    syncOptions:
    - FailOnSharedResource=true
```

### Create Namespace

If we want to create the namespace when it does not exist, we can set the `syncOptions` to `CreateNamespace=true`. By default, Argo CD does not create the namespace when it does not exist.

**At Application Level:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  namespace: argocd
spec:
  destination:
    server: https://kubernetes.default.svc
    namespace: some-namespace
  syncPolicy:
    syncOptions:
    - CreateNamespace=true
```

Date of Creation: 27/04/2024