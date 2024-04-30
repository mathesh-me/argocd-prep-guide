## ApplicationSets

**ApplicationSets** are used to **automate the generation of Argo CD Applications**. It allows us to define a set of applications that can be generated from a template. 

**Best Use Cases:**

- When we have **multiple applications to be deployed in the same way**.
- When we have **multiple applications to be deployed in the same way but with different configurations.**
- To deploy a **Same application in multiple clusters**.
- To deploy **multiple applications in multiple clusters**.
- **Multitenancy support**, improves the ability of individual cluster tenants to deploy applications using Argo CD. **Ex:** developers can deploy services into specific cluster/namespace without engaging clusters admins.

**All the above things can be done using a Single manifest file.**

### ApplicationSets Working

- It will not deploy the applications directly. Instead, it will generate the **application** manifests.
- We can use ApplicationSets to Create, Update, and Delete the applications.

### How to Create ApplicationSets?

We can create ApplicationSets using the K8S manifest file.

**Example:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: my-appset
  namespace: argocd
spec:
  generators:
  - list:
      elements:
      - name: my-app1
        namespace: default
      - name: my-app2
        namespace: default
  template:
    metadata:
      labels:
        app.kubernetes.io/name: "{{name}}"
    spec:
      project: default
      source:
        repoURL: https://github.com/argoproj/argocd-example-apps.git
        targetRevision: HEAD
        path: "{{name}}"
      destination:
        server: https://kubernetes.default.svc
        namespace: "{{namespace}}"
      syncPolicy:
        automated: {}
```

Date of Creation: 27/04/2024
