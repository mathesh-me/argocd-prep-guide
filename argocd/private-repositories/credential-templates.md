## Credential Templates

Useful when we want to use a **single credential for multiple repositories**. We can create a credential template and then use it in multiple repositories.<br>

Example Use case: Organization has multiple repositories and we want to use the same credential for all the repositories.

### How to Create Credential Templates?

- Same as adding private Git repositories, we have to add credential templates as K8S secret. Just need to change the label `argocd.argoproj.io/secret-type` to `repo-creds`.
- And one more thing, If we want to use this credential template for any repo, That repo must not configured or must not have any credentials.
- We can add our Credential Templates in the below ways:
    - Using Declartive way
    - Using Argo CD CLI
    - Using Argo CD UI

#### Declartive Way:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-credential-template-secret
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repo-creds
stringData:
  type: git
  url: https://github.com/matheshme
  username: my-username
  password: my-password
```

#### How to Add Credential Templates using Argo CD CLI?

Use the below Command:

```bash
argocd repocreds add https://git.example.com/repos/ --username git -
-password secret
```

#### How to Add Credential Templates using Argo CD UI?

- Go to the Argo CD UI.
- Click on the `Settings` tab.
- Click on the `Repositories` tab.
- Click on the `Add Repository` button.
- Fill the details and Click `Save as credential template`.

Date of Notes: 27/04/2024