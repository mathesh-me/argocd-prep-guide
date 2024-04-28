## Sync Phases and Waves

### Sync Phases

Actually there are 3 phases in the sync process:

1. **PreSync**: It is the first phase of the sync process. It is used to prepare. **Example:** We can have DB migrations in this phase.
2. **Sync**: This is the main phase of the sync process. It is used to apply the changes to the cluster. **Example:** We can have the actual deployment in this phase.
3. **PostSync**: It is the last phase of the sync process. **Example:** Sending notifications.

We can describe `Phases` using **Resource Hooks** in the application manifest file.

**Example of using Resource Hooks:**

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
  annotations:
    argocd.argoproj.io/hook: PreSync
spec:
   <job-spec>
```
**Options of Resource Hooks:**

- **PreSync**: It is executed before the sync process.
- **Sync**: It is executed during the sync process.
- **PostSync**: It is executed after the sync process.
- **Skip**: It is used to skip the resource from the sync process.
- **SyncFail**: Executes after the sync operation fails.

#### Hook Deletion Policy

We can set the `hook-delete-policy` field in the application manifest file to control the deletion of the hook resources.

**Options of Hook Deletion Policy:**

- **BeforeHookCreation**: It deletes the hook resources before creating new resources. Better for Sync Phase.
- **HookSucceeded**: It deletes the hook resources after the hook resources are executed successfully. Better for PreSync Phase.
- **HookFailed**: It deletes the hook resources after the hook resources are failed. Better for PostSync Phase.

**Example of using Hook Deletion Policy:**

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
  annotations:
    argocd.argoproj.io/hook: PreSync
    argocd.argoproj.io/hook-delete-policy: HookSucceeded
spec:
  <job-spec>
```
---

### Sync Waves

Sync Waves are used to control the order of the sync process. We can set the `wave` field in the application manifest file to control the order of the sync process. By default, **the wave is set to 0**. **Lesser will be executed first**. We can use both positive and negative numbers.

**Example of using Sync Waves:**

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
  annotations:
    argocd.argoproj.io/sync-wave: "1"
spec:
  <job-spec>
```

- The above example sets the wave to 1. So it will be executed after the resources with wave 0. 
- Each wave will have 2 seconds of delay between them.

---

### Argo CD Syncing Priority

1. Phases
2. Waves
3. Resource Kind
4. Resource Name

Date of Creation: 27/04/2024