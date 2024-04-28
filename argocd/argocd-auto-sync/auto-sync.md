## Auto Sync in Argo CD

We can enable auto-sync in Argo CD to **automatically sync the applications whenever there is a change in the application source repository**. Argo CD will poll the git repo for every **3 minutes** and sync the application if there is any change in the source repository.<br>

**Note:** Auto-sync is **disabled by default** in Argo CD. Auto-sync will not reattempt the sync if the previous sync failed for same Commit SHA and Parameters. And also, Rollback cannot be performed if we enable auto-sync. 

### How to Enable Auto Sync?

There are three ways to enable auto-sync in Argo CD:

- **Using Declarative Way**: We can enable auto-sync in Argo CD using the K8S manifest file.
- **Using Argo CD UI**: We can enable auto-sync in Argo CD using the Argo CD UI.
- **Using Argo CD CLI**: We can enable auto-sync in Argo CD using the Argo CD CLI.

#### Declarative Way:

Create an Application manifest file and add the `syncPolicy` field to enable auto-sync.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD
    path: my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated: {}
```

#### How to Enable Auto Sync using Argo CD CLI?

Use the below Command:

```bash
argocd app set my-app --sync-policy automated <other-flags>
```

#### How to Enable Auto Sync using Argo CD UI?

- Navigate to the Argo CD UI.
- Click on the `Applications` tab.
- Click on the Application.
- Click on the `Settings` tab.
- Just Enable the `Auto-Sync` toggle button.

---

## Auto Prune in Argo CD

Let's say **we enabled auto-sync in Argo CD. And Argo CD synced the application with the source repository. If we delete any resource from the source repository now, that resource will not be deleted from the cluster. To delete the resources from the cluster, we have to enable auto-prune in Argo CD**. By default, auto-prune is disabled in Argo CD.

### How to Enable Auto Prune?

The same as enabling auto-sync, we can enable auto-prune in Argo CD using the K8S manifest file, Argo CD UI, and Argo CD CLI.

#### Declarative Way:

Create an Application manifest file and add the `syncPolicy` field to enable auto-prune.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD
    path: my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
```

#### How to Enable Auto Prune using Argo CD CLI?

Use the below Command:

```bash
argocd app set my-app --sync-policy automated --sync-option Prune=true <other-flags>
```

#### How to Enable Auto Prune using Argo CD UI?

- Follow the above methods I mentioned for enabling auto-sync.
- After that, You will be prompted with the `Auto Prune` toggle button. Enable it.

---

## Self Heal in Argo CD

If **we edit some configuration in the cluster manually, Argo CD will not sync it automatically. To sync the changes, we have to enable self-heal in Argo CD.** Self-heal is disabled by default in Argo CD. So, Argo CD will always maintain the desired state of the application.

### How to Enable Self Heal?

The same three ways we followed for every approach, we can enable self-heal in Argo CD using the K8S manifest file, Argo CD UI, and Argo CD CLI.

#### Declarative Way:

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
    automated:
      selfHeal: true
      prune: true
```

#### How to Enable Self Heal using Argo CD CLI?

Use the below Command:

```bash
argocd app set my-app --sync-policy automated --self-heal <other-flags>
```

#### How to Enable Self Heal using Argo CD UI?

- Follow the above methods I mentioned for enabling auto-sync.
- After that, You will be prompted with the `Self Heal` toggle button. Enable it.

Date of Notes: 27/04/2024