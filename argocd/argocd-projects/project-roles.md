## Project Roles

### How to Create Project Roles?

We can create role with set of permissions to grant access to the project applications.

- We can create a role for CI agents a specific access to project applications(Must be Associated with JWT Tokens)
- We can also create roles for OIDC groups.

**Example of a role manifest file:**

```yaml
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata: 
  name: myproject
  namespace: argocd
spec: 
  description: project description 
  sourceRepos:
  - “*”
  destinations: 
  - server: “*”
    namespace: “*“
  clusterResourceWhitelist: 
  - group: “*”
    kind: “*“
  roles:
  - name: ci-role
    description: Sync privileges for demo-project
    policies:
    - p, proj:myproject:ci-role, applications, sync, myproject/*, allow
```

The above example creates a role `ci-role` with the following permissions:

- `applications` - Sync privileges for `myproject/*`

Date of Creation: 27/04/2024