## Argo CD Projects

### What is Project?

A project is a **collection of applications**. We can group applications into projects. We can use Projects to define access control policies for applications. A Default project is created when Argo CD is installed. 

**Access Restrictions in Project:**

It will be useful **when we have multiple teams** working on different applications. We can restrict access to applications based on the project.

- **Trusted Git Repository**: We can allow only the trusted Repo.
- **Destination Cluster and Namespace**: We can allow apps to be deployed only in a specific cluster and namespace.
- **Specific Resources**: We can allow only specific resources to be deployed.

**Project Roles:**

We can create role with set of permissions to grant access to the project applications.

- We can create a role for CI agents a specific access to project applications(Must be Associated with JWT Tokens)
- We can also create roles for OIDC groups.

### How to Create Project?

Three ways of Creating Argo CD Project:

- Declative way using K8S Manifest
- Using Argo CD UI
- Using Argo CD CLI

#### Declarative Way:

Create a project manifest file and apply it to the cluster.

1. Allowing all the resources to be deployed in all the clusters and namespaces.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: myproject
  namespace: argocd
spec:
  destinations:
    - namespace: '*'
      server: '*'
  sourceRepos:
    - '*'
  clusterResourceWhitelist:
    - group: '*'
      kind: '*'
  namespaceResourceWhitelist:
    - group: '*'
      kind: '*'
```

2. Allowing only specific resources to be deployed in specific clusters and namespaces and only specific repositories as source

```yaml
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: myproject
  namespace: argocd
spec:
  destinations:
    - namespace: 'default'
      server: 'https://kubernetes.default.svc'
  sourceRepos:
    - 'https://github.com/argoproj/argocd-example-apps.git'
  clusterResourceWhitelist:
    - group: ''
      kind: 'Namespace'
  namespaceResourceWhitelist:
    - group: 'apps'
      kind: 'Deployment'
  namespaceResourceBlacklist:
    - group: ''
      kind: 'Service'
```
---

#### Using Argo CD UI:

1. Click on the `Projects` tab on the left side menu.
2. Click on the `New Project` button.
3. Fill in the project details and click on the `Create` button.
![Screenshot 2024-04-28 195518](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/08389b64-7cf8-4c2a-8496-66b84aa99ca5)
![Screenshot 2024-04-28 195541](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/9c6e6612-c574-493c-a156-6596c6f83619)
![Screenshot 2024-04-28 195638](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/b2d8fc39-59e5-48ec-8423-f7b609a420f4)
**You can leave Every section as it is, But note that it's allow every Repo to be source, All K8S resources are allowed to be deployed and We can also deploy to any K8S Clusters. So if You want some restrictions, Just edit the apppropriate section like below:**
![Screenshot 2024-04-28 195654](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/249c565f-c344-4c29-85c4-ee5b4840dc6f)
![Screenshot 2024-04-28 195736](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/68b47295-eab1-4b03-8775-10e82f710236)
![Screenshot 2024-04-28 195752](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/6c22b495-b1c2-4c78-be5b-4749028bf1ad)

---

#### Using Argo CD CLI:

```bash
argocd proj create myproject <flags>
```

Refer Argo CD official [Documentation](https://argo-cd.readthedocs.io/en/latest/user-guide/commands/argocd_proj_create/) for Flags and Options.

---

### Set Project Details in Application:

We can set the project details in the application manifest file.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: myapp
  namespace: argocd
spec:
  project: myproject
  source:
    repoURL: https://argo-cd.readthedocs.io/en/latest/user-guide/commands/argocd_proj_create/
    targetRevision: HEAD
    path: guestbook
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

Date of Notes: 27/04/2024
