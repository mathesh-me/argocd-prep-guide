## Argo CD Projects

### What is Project?

A project is a collection of applications. We can group applications into projects. We can use Projects to define access control policies for applications. A Default project is created when Argo CD is installed. 

**Access Restrictions in Project:**

It will be useful when we have multiple teams working on different applications. We can restrict access to applications based on the project.

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

#### Using Argo CD UI:

1. Click on the `Projects` tab on the left side menu.
2. Click on the `New Project` button.
3. Fill in the project details and click on the `Create` button.

#### Using Argo CD CLI:

```bash
argocd proj create myproject <flags>
```

Refer Argo CD official [Documentation](https://argo-cd.readthedocs.io/en/latest/user-guide/commands/argocd_proj_create/) for Flags and Options.

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