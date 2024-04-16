## GitOps

### What is GitOps?

GitOps is a way to do Continuous Delivery. It uses ***Git as a single source of truth for declarative infrastructure and applications***.

### Why GitOps?

Let's say we have a Kubernetes cluster and Someone Modify some configurations in the cluster. How do we know what has changed? How do we know who made the change? How do we know when the change was made?. **So the Idea is like If we can have Version Control System like Git to store Application Source Code, Why not use Git to store the configuration of the Kubernetes Cluster**. So GitOps is a way to do Continuous Delivery for Applications and Infrastructure using Git.

### GitOps Principles

- **Declarative**: A system managed by **GitOps must have its desired state expressed declaratively**. It means The desired state of the system have to be  declared in configuration files stored in Git.
- **Versioned**: Desired state is stored in a way that enforces immutability, versioning and retains a complete version history. All changes to the system are **Versioned in Git**, enabling rollbacks and audits.
- **Automated**: Software agents automatically pull the desired state declarations from the source. Changes will be **automatically applied** and verified by a GitOps tool.
- **Secure**: Software agents continuously observe actual system state and attempt to apply the desired state. So any **Manual changes to the system will be overwritten by the desired state**.

### Is GitOps only for Kubernetes?

No, GitOps is not only for Kubernetes. GitOps is a way to do Continuous Delivery for Applications and Infrastructure using Git. GitOps can be used for any Infrastructure like AWS, GCP, Azure, etc.

But Popular GitOps tools like ArgoCD, Flux, JenkinsX are mainly used for Kubernetes.

### Is GitOps Only works with Git?

No, GitOps isn't limited to Git specifically. Git is the most common version control system (VCS) used with GitOps, **the core principle can be applied with other VCS tools that offer similar functionalities.**
 
### How GitOps Works?

Whenever there is a change in the Git Repository, the GitOps tool will automatically apply the changes to the Kubernetes Cluster. **The GitOps tool will continuously monitor the Git Repository for any changes and apply the changes to the Kubernetes Cluster**. It Can also be Push based where we can set up a Webhook in the Git Repository and whenever there is a change in the Git Repository, the Webhook will trigger the GitOps tool to apply the changes to the Kubernetes Cluster.

### Popular GitOps Tools

- **ArgoCD**: ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes.
- **Flux**: Flux is a tool that automatically ensures that the state of a cluster matches the config in git.
- **JenkinsX**: Jenkins X is a CI/CD solution for modern cloud applications on Kubernetes.
- **Spinnaker**: Spinnaker is an open-source, multi-cloud continuous delivery platform for releasing software changes with high velocity and confidence.

#### Advantages of GitOps

- **Security**: GitOps ensures that the system is always in the desired state. Any manual changes to the system will be `overwritten` by the desired state.
- **Reconciliation**: GitOps continuously monitors the actual state of the system and attempts to apply the desired state. If there is any drift between the actual state and the desired state, GitOps will automatically reconcile the drift.
- **Versioning**: All changes to the system are `versioned`in Git, enabling rollbacks and audits.
- **Auto Updates**: GitOps automatically updates the system whenever there is a change in the Git Repository.

Date of Notes: 16/04/2024