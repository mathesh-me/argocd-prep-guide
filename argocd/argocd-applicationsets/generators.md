## Generators in Argocd ApplicationSets

Generators in ArgoCD ApplicationSets are used for g**enerating Parameters which is then further to generate Applications templates**. 

**Types of Generators:**

- **List Generator**
- **Cluster Generator**
- **Git Generator**
- **Matrix Generator**
- **Merge Generator**
- **SCM Provider Generator**
- **Pull Request Generator**
- **Cluster Decision Resource Generator**

---

### List Generator

The List generator allows you to target Argo CD Applications to clusters based on a **fixed list of any chosen key/value element pairs**. It generates parameters based on an arbitrary list of key/value pairs

**Example of List Generator:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: guestbook
  namespace: argocd
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
  - list:
      elements:
      - cluster: engineering-dev
        url: https://kubernetes.default.svc
      - cluster: engineering-prod
        url: https://kubernetes.default.svc
  template:
    metadata:
      name: '{{.cluster}}-guestbook'
    spec:
      project: "my-project"
      source:
        repoURL: https://github.com/argoproj/argo-cd.git
        targetRevision: HEAD
        path: applicationset/examples/list-generator/guestbook/{{.cluster}}
      destination:
        server: '{{.url}}'
        namespace: guestbook
```
---

### Cluster Generator

The Cluster generator allows us to target Argo CD Applications to clusters, **based on the list of clusters defined within (and managed by) Argo CD**(which includes automatically responding to cluster addition/removal events from Argo CD). But for this we already need to have the clusters added to the Argo CD.<br>

The Cluster generator will **automatically identify** clusters defined with Argo CD, and extract the cluster data as parameters.

1. Example of Cluster Generator for **all Clusters**:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: guestbook
  namespace: argocd
spec:
  generators:
  - clusters: {} # Automatically use all clusters defined within Argo CD
  template:
    metadata:
      name: '{{.name}}-guestbook' # 'name' field of the Secret
    spec:
      project: "my-project"
      source:
        repoURL: https://github.com/argoproj/argocd-example-apps/
        targetRevision: HEAD
        path: guestbook
      destination:
        server: '{{.server}}' # 'server' field of the secret
        namespace: guestbook
```
The above example will generate Applications for all the clusters defined in the Argo CD.

2. Example of Cluster Generator with **specific Clusters**:

```yaml
kind: ApplicationSet
metadata:
  name: guestbook
  namespace: argocd
spec:
  generators:
  - clusters:
      selector:
        matchLabels:
          staging: true
        # The cluster generator also supports matchExpressions.
        #matchExpressions:
        #  - key: staging
        #    operator: In
        #    values:
        #      - "true"
  template:
  # (...)
```
The above example will generate Applications for the clusters which have the label `staging: true`. For adding the label to the cluster, we have to add it in **Cluster K8S Secret**.

**Note:** By default local cluster is not added as k8s secret, We can add it declaratively or using Web UI by editing the cluster name in clusters page. Then We have to it.

---

### Git Generator

- The Git generator allows you to create Applications based on **files within a Git repository, or based on the directory structure** of a Git repository.
- The Git generator contains **two subtypes**: the Git directory generator, and Git file generator.
- **Git Directory Generator**: It generates Applications based on the directory structure of a Git repository.
- **Git File Generator**: The Git file generator generates parameters using the contents of JSON/YAML files found within a specified repository.
- **The generator parameters are**:
    - {{.path.path}}: The directory paths within the Git repository that match the path wildcard.
    - {{index .path.segments n}}: The directory paths within the Git repository that match the path wildcard, split into array elements (n - array index)
    - {{.path.basename}}: For any directory path within the Git repository that matches the path wildcard, the right-most path name is extracted (e.g. /directory/directory2 would produce directory2).
    - {{.path.basenameNormalized}}: This field is the same as path.basename with unsupported characters replaced with - (e.g. a path of /directory/directory_2, and path.basename of directory_2 would produce directory-2 here).
Note: The right-most path name always becomes {{.path.basename}}. For example, for - path: /one/two/three/four, {{.path.basename}} is four.

#### Git Directory Generator

**Example of Git Directory Generator:**

It generates Applications based on the **directory structure** of a Git repository.

Suppose we have a Git repository with the following directory structure:

```
├── argo-workflows
│   ├── kustomization.yaml
│   └── namespace-install.yaml
└── prometheus-operator
    ├── Chart.yaml
    ├── README.md
    ├── requirements.yaml
    └── values.yaml
```
This repository contains two directories, one for each of the workloads to deploy:<br>
1. an Argo Workflow controller kustomization YAML file
2. a Prometheus Operator Helm chart

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: cluster-addons
  namespace: argocd
spec:
  generators:
  - git:
      repoURL: https://github.com/argoproj/argo-cd.git
      revision: HEAD
      directories:
      - path: applicationset/examples/git-generator-directory/cluster-addons/*
  template:
    metadata:
      name: '{{.path.basename}}'
    spec:
      project: "my-project"
      source:
        repoURL: https://github.com/argoproj/argo-cd.git
        targetRevision: HEAD
        path: '{{.path.path}}'
      destination:
        server: https://kubernetes.default.svc
        namespace: '{{.path.basename}}'
      syncPolicy:
        syncOptions:
        - CreateNamespace=true
```

**Example of Git Directory Generator with Exclude:**

The Git directory generator will automatically exclude directories that begin with . (such as .git).<br>

The Git directory generator also supports an exclude option in order to exclude directories in the repository from being scanned by the ApplicationSet controller:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: cluster-addons
  namespace: argocd
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
  - git:
      repoURL: https://github.com/argoproj/argo-cd.git
      revision: HEAD
      directories:
      - path: applicationset/examples/git-generator-directory/excludes/cluster-addons/*
      - path: applicationset/examples/git-generator-directory/excludes/cluster-addons/exclude-helm-guestbook
        exclude: true
  template:
    metadata:
      name: '{{.path.basename}}'
    spec:
      project: "my-project"
      source:
        repoURL: https://github.com/argoproj/argo-cd.git
        targetRevision: HEAD
        path: '{{.path.path}}'
      destination:
        server: https://kubernetes.default.svc
        namespace: '{{.path.basename}}'
```

#### Git File Generator

- The Git file generator generates parameters using the **contents of JSON/YAML files** found within a specified repository.
- Suppose we have a Git repository with the following directory structure:

```yaml
├── apps
│   └── guestbook
│       ├── guestbook-ui-deployment.yaml
│       ├── guestbook-ui-svc.yaml
│       └── kustomization.yaml
├── cluster-config
│   └── engineering
│       ├── dev
│       │   └── config.json
│       └── prod
│           └── config.json
└── git-generator-files.yaml
```

The config.json files contain information describing the cluster (along with extra sample data):

```json
{
  "aws_account": "123456",
  "asset_id": "11223344",
  "cluster": {
    "owner": "cluster-admin@company.com",
    "name": "engineering-dev",
    "address": "https://1.2.3.4"
  }
}
```

We can Create our ApplicationSet Manifest as below:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: guestbook
  namespace: argocd
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
  - git:
      repoURL: https://github.com/argoproj/argo-cd.git
      revision: HEAD
      files:
      - path: "applicationset/examples/git-generator-files-discovery/cluster-config/**/config.json"
  template:
    metadata:
      name: '{{.cluster.name}}-guestbook'
    spec:
      project: default
      source:
        repoURL: https://github.com/argoproj/argo-cd.git
        targetRevision: HEAD
        path: "applicationset/examples/git-generator-files-discovery/apps/guestbook"
      destination:
        server: '{{.cluster.address}}'
        namespace: guestbook
```
---

### Matrix Generator

The Matrix generator may be used to **combine the generated parameters of two separate generators**. The Matrix generator combines the parameters generated by two child generators, iterating through every combination of each generator's generated parameters.<br>

The Best Use case is **Git Directory Generator + Cluster Decision Resource Generator**: Locate application resources contained within folders of a Git repository, and deploy them to a list of clusters provided via an external custom resource.<br>

**Note:**If both child generators are Git generators, one or both of them must use the pathParamPrefix option to avoid conflicts when merging the child generators’ items.

**Example of Matrix Generator:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: cluster-git
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
    # matrix 'parent' generator
    - matrix:
        generators:
          # git generator, 'child' #1
          - git:
              repoURL: https://github.com/argoproj/argo-cd.git
              revision: HEAD
              directories:
                - path: applicationset/examples/matrix/cluster-addons/*
          # cluster generator, 'child' #2
          - clusters:
              selector:
                matchLabels:
                  argocd.argoproj.io/secret-type: cluster
  template:
    metadata:
      name: '{{.path.basename}}-{{.name}}'
    spec:
      project: '{{index .metadata.labels "environment"}}'
      source:
        repoURL: https://github.com/argoproj/argo-cd.git
        targetRevision: HEAD
        path: '{{.path.path}}'
      destination:
        server: '{{.server}}'
        namespace: '{{.path.basename}}'
```

The above example will generate application for all clusters based on the directories in the Git repository. For instnace let's say we have the following directories in the Git repository:

```
├── cluster-addons
│   ├── argo-workflows
│   │   ├── kustomization.yaml
│   │   └── namespace-install.yaml
│   └── prometheus-operator
│       ├── Chart.yaml
│       ├── README.md
│       ├── requirements.yaml
│       └── values.yaml
```

It will genrate two applications for each cluster based on the directories in the Git repository.

---

### Pull Request Generator

The Pull Request generator uses the API of an SCMaaS provider (GitHub, Gitea, or Bitbucket Server) to automatically discover open pull requests within a repository. This fits well with the style of building a test environment when you create a pull request.

**Example of Pull Request Generator USING GitHub:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: myapps
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
  - pullRequest:
      github:
        # The GitHub organization or user.
        owner: myorg
        # The Github repository
        repo: myrepository
        # For GitHub Enterprise (optional)
        api: https://git.example.com/
        # Reference to a Secret containing an access token. (optional)
        tokenRef:
          secretName: github-token
          key: token
        # (optional) use a GitHub App to access the API instead of a PAT.
        appSecretName: github-app-repo-creds
        # Labels is used to filter the PRs that you want to target. (optional)
        labels:
        - preview
      requeueAfterSeconds: 1800
  template:
    metadata:
      name: 'myapp-{{.branch}}-{{.number}}'
    spec:
      source:
        repoURL: 'https://github.com/myorg/myrepo.git'
        targetRevision: '{{.head_sha}}'
        path: kubernetes/
        helm:
          parameters:
          - name: "image.tag"
            value: "pull-{{.head_sha}}"
      project: "my-project"
      destination:
        server: https://kubernetes.default.svc
        namespace: default

```

**Available Parameters:**

- number: The ID number of the pull request.
- branch: The name of the branch of the pull request head.
- branch_slug: The branch name will be cleaned to be conform to the DNS label standard as defined in RFC 1123, and truncated to 50 characters to give room to append/suffix-ing it with 13 more characters.
- target_branch: The name of the target branch of the pull request.
- target_branch_slug: The target branch name will be cleaned to be conform to the DNS label standard as defined in RFC 1123, and truncated to 50 characters to give room to append/suffix-ing it with 13 more characters.
- head_sha: This is the SHA of the head of the pull request.
- head_short_sha: This is the short SHA of the head of the pull request (8 characters long or the length of the head SHA if it's shorter).
- head_short_sha_7: This is the short SHA of the head of the pull request (7 characters long or the length of the head SHA if it's shorter).
- labels: The array of pull request labels. (Supported only for Go Template ApplicationSet manifests.)

**Filters**
Filters allow selecting which pull requests to generate for. Each filter can declare one or more conditions, all of which must pass. If multiple filters are present, any can match for a repository to be included. If no filters are specified, all pull requests will be processed. Currently, only a subset of filters is available when comparing with SCM provider filters.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: myapps
spec:
  goTemplate: true
  goTemplateOptions: ["missingkey=error"]
  generators:
  - pullRequest:
      # ...
      # Include any pull request ending with "argocd". (optional)
      filters:
      - branchMatch: ".*-argocd"
  template:
  # ...
```
**The available filters are:**
- `branchMatch` A regexp matched against source branch names.
- `targetBranchMatch`: A regexp matched against target branch names.
GitHub and GitLab also support a **labels** filter.

---

### Other Generators

- **SCM Provider generator**: The SCM Provider generator uses the API of an SCM provider (eg GitHub) to automatically discover repositories within an organization.
- **Cluster Decision Resource generator**: The Cluster Decision Resource generator is used to interface with Kubernetes custom resources that use custom resource-specific logic to decide which set of Argo CD clusters to deploy to.
- **Plugin generator**: The Plugin generator make RPC HTTP request to provide parameters.

Date of Notes: 28/04/2024