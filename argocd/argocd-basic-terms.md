## Basic Terms in Argo CD

### Application

An application is a collection of Kubernetes resources that are deployed together. An application can be defined in a Git repository. Argo CD will continuously monitor the desired application state defined in the Git repository and compare it with the live state of the application running on the Kubernetes cluster. If there is any drift between the desired state and the live state, Argo CD will automatically reconcile the drift.

### Project

A project is a collection of applications. We can group applications into projects. We can use Projects to define access control policies for applications.

### Actual State vs Desired State

- **Actual State**: The actual state of the application running on the Kubernetes cluster.
- **Desired State**: The desired state of the application defined in the Git repository.

### Sync

Sync is like a process of comparing the actual state of the application running on the Kubernetes cluster with the desired state defined in the Git repository. If there is any drift between the actual state and the desired state, Argo CD will automatically reconcile the drift.

### Refresh

- Compare the latest code in Git with the live state. Figure out what is different.
- ArgoCD automatically refreshes every 3 minutes

Date of Creation: 26/04/2024