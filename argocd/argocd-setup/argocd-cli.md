## Argo CD CLI:

### How to Install Argo CD CLI?

Download `latest version`:

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

Download `concrete version`:
Set **VERSION** replacing <TAG> in the command below with the version of Argo CD you would like to download:<br>

VERSION=<TAG> # Select desired TAG from https://github.com/argoproj/argo-cd/releases

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

### How to Login to Argo CD using CLI?

Use the below Command:

```bash
argocd login <ARGOCD_SERVER> --username <USERNAME> --password <PASSWORD>
```

### Basic Commands:

1. **List Applications:**

```bash
argocd app list
```

2. **Get Application Details:**

```bash
argocd app get <APP_NAME>
```

3. **Sync Application:**

```bash
argocd app sync <APP_NAME>
```

4. **Delete Application:**

```bash
argocd app delete <APP_NAME>
```

5. **List Projects:**

```bash
argocd proj list
```

6. **Get Project Details:**

```bash
argocd proj get <PROJECT_NAME>
```

7. **Create Project:**

```bash
argocd proj create <PROJECT_NAME>
```

8. **Delete Project:**

```bash
argocd proj delete <PROJECT_NAME>
```

9. **List Repositories:**

```bash
argocd repo list
```

10. **Get Repository Details:**

```bash
argocd repo get <REPO_URL>
```

Date of Notes: 28/04/2024