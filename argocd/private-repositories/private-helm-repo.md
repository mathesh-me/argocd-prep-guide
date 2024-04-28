## Private Helm Repository

We have to register our private Helm repositories in Argo CD to deploy the applications from the private Helm repositories.

### How to Add Private Helm Repositories?

- Same as adding private Git repositories, we have to **add private Helm repositories as K8S secret**.

We can add our creds using the following ways:

- **Using Username/Password and tls Cert/Key**

**Example of adding Private Helm Repositories as K8S Secret using Declarative way:**

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
  type: helm
  name: my-repo
  username: my-user
  password: <helm
  tlsClientCertData: <tls-client-cert-data>
  tlsClientCertKey: <tls-client-cert-key>
```

### How to Add Private Helm Repositories using Argo CD CLI?

Use the below Command:

```bash
argocd repo add <repo-url> --type helm --name <repo-name> --username <username> --password <password>
```

### How to Add Private Helm Repositories using Argo CD UI?

- Navigate to the Argo CD UI.
- Click on the `Settings` tab.
- Click on the `Repositories` tab.
- Click on the `Add Repository` button.
- Fill the details and click on the `Save` button.

Date of Notes: 27/04/2024