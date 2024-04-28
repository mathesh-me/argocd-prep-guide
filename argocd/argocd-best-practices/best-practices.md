## Best Practices in Argo CD

- Having **separate repository for Configurations** than the Application code is a good practice.
- Using **Immutable manifests** in Production like instead of tracking git repo using `HEAD` use a specific commit hash or stable tag.
- Ignore replicas in the manifests, If we want to use HPA.
- Use **Sealed Secrets, SOP and External Secrets manager** to store sensitive data. Don't store plain secret in Git.
- Use a **Separate Argo CD instance for production** than the non-prod Environments.(Use HA for production)
- Use `App of Apps` to manage all the applications in the cluster.
- Use `ApplicationSets` to manage multiple applications in the multiple clusters.

Date of Creation: 28/04/2024