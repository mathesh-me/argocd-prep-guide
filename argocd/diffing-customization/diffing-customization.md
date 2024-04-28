## Diffing Customization

### Why we need diffing customization?

- It is possible for an application to be **OutOfSync** even immediately after a successful Sync operation. Some reasons may be:
    - There is a bug in the manifest, where it contains extra/unknown fields from the actual K8s spec. These extra fields would get dropped when querying Kubernetes for the live state, resulting in an OutOfSync status indicating a missing field was detected.
    - The sync was performed (with pruning disabled), and there are resources which need to be deleted.
    - A controller or mutating webhook is altering the object after it was submitted to Kubernetes in a manner which contradicts Git.

- Diffing Customization can be configured single or multiple application resources or at a system level.

#### Ignoring differences(Application Level)

- We can ignore the differences using the below options:
    - **RFC6902 JSON Patch**: We can ignore the differences using the RFC6902 JSON Patch.
    - **JQ Path Expression**: We can ignore the differences using the JQ Path Expression.
    - **By Specific Controllers**: We can ignore the differences by specific controllers.

- **Example of ignoring differences using RFC6902 JSON Patch:**

```yaml
spec:
  ignoreDifferences:
  - group: apps
    kind: Deployment
    # We can also specify the Object Name and Namespace
    # name: my-deployment
    # namespace: my-namespace 
    jsonPointers:
    - /spec/replicas
```

- **Example of ignoring differences using JQ Path Expression:**

```yaml
spec:
  ignoreDifferences:
  - group: apps
    kind: Deployment
    jqPathExpressions:
    - .spec.template.spec.initContainers[] | select(.name == "injected-init-container")
```

- **Example of ignoring differences by specific controllers:**

```yaml
spec:
  ignoreDifferences:
  - group: "*"
    kind: "*"
    managedFieldsManagers:
    - kube-controller-manager
```

The above configuration will ignore differences from all fields owned by kube-controller-manager for all resources belonging to this application.<br>

If you have a slash / in your pointer path, you can use the ~1 character. For example:

```yaml
spec:
  ignoreDifferences:
  - kind: Node
    jsonPointers: /metadata/labels/node-role.kubernetes.io~1worker
```

Date of Notes: 27/04/2024