## Adding a remote cluster to ArgoCD

By default Argo CD have permissions to manage resources in the cluster where it is installed. If you want to manage resources in a remote cluster, you need to add the remote cluster to Argo CD.

### How to Add a Remote Cluster?

1. **Declarative Way:**
2. **Using Argo CD UI:**
3. **Using Argo CD CLI:**

#### Declarative Way:

- We have to add our Cluster credentials as `K8S Secret` in the Argo CD.
- In that way our Cluster can register with the Argo CD.
- We must have to add the `argocd.argoproj.io/secret-type: cluster` label to the K8S Secret.
- That `K8S Secret` must have the following fields:
  - `name`: Name of the cluster.
  - `server`: URL of the cluster.
  - `config`: Configuration of the cluster.
- Below are the Options to add the Remote Cluster:

1. **Using Username/Password and tls Cert/Key**
2. **Using Bearer Token**
3. **Using IAM in case of Cloud Providers**
4. **Using External Providers to supply Credentials**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-remote-cluster-secret
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: cluster
stringData:
  name: my-remote-cluster
  server: https://my-remote-cluster:6443
  config: |
    {
        "bearer
        token
        ":
        "my
        -token
        "
    }
```

#### How to Add Remote Cluster using Argo CD CLI?

Use the below Command:

```bash
argocd cluster add my-remote-cluster --kubeconfig <kubeconfig-path>
```

#### How to Add Remote Cluster using Argo CD UI?

- Navigate to the Argo CD UI.
- Click on the `Settings` tab.
- Click on the `Clusters` tab.
- Click on the `Add Cluster` button.
- Fill the details and click on the `Save` button.

Date of Notes: 27/04/2024