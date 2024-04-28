## Private Git Repositories

Normally, we use public repositories for our applications. But sometimes, we need to use private repositories for our applications. In this case, **we need to provide the credentials to access the private repositories**. We can provide the credentials in the following ways:

- **Using SSH Keys**: We can use SSH keys to access the private repositories.
- **Using GitHub Apps**: We can use Personal Access Tokens to access the private repositories.
- **Using Username and Password**: We can use Username and Password to access the private repositories.

### How to Add Private Git Repositories?

- We have to add the private repositories in the Argo CD.
- We need to create a **K8S secret** with the `repo` credentials for registering the private repositories.
- We can add our Private Repositories in the Argo CD using Declartive way, Argo CD UI way or Argo CD CLI way.

**Example of adding Private Repositories as K8S Secret using Declarative way:**

1. Using Username and Password:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-private-repo-secret
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  url: https://github.com/argoproj/argocd-example-apps.git
  type: git
  username: my-token
  password: <git-access-token>
  # In case we are going with Insecure connection
  # insecure: "true" 
  # In case we are going with Secure connection
  # tlsClientCertData: <tls-client-cert-data>
  # tlsClientCertKey: <tls-client-cert-key>
```

2. Using SSH Keys:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-private-repo-secret
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  url: https://github.com/argoproj/argocd-example-apps.git
  type: git
  sshPrivateKey: |
    -----BEGIN
    <private
    key>
    -----END
```

3. Using GitHub Apps:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-private-repo-secret
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  url: https://github.com/argoproj/argocd-example-apps.git
  type: git
  githubAppID: <app-id>
  githubAppInstallationID: <installation-id>
  githubAppPrivateKey: |
    -----BEGIN
    <private
    key
    >
    -----END
```

### How to Add Private Git Repositories using Argo CD CLI?

Use the below Command:

```bash
argocd repo add <repo-url> --type git --username <username> --password <password>
```

### How to Add Private Git Repositories using Argo CD UI?

- Go to the Argo CD UI.
- Click on the `Settings` tab.
- Click on the `Repositories` tab.
- Click on the `Add Repository` button.
- Fill the details and click on the `Save` button.

Date of Notes: 27/04/2024