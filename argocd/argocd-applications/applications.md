## Argo CD Applications

### What is an Argo CD Application?

The Application CRD is the **Kubernetes resource object** representing a deployed application instance in an environment.

**An application is defined by two main components:**

1. **Source**: The source of the application is defined in the **Git repository**. It will contains our Kubernetes manifests that define the application.
2. **Destination**: The destination is the **target environment** where we want to deploy our Application. It contains the Kubernetes cluster URL and the namespace where the application will be deployed.

### How to Create an Application in Argo CD?

We have three ways to create an application in Argo CD:

1. **Web UI**: We can create an application using the Argo CD Web UI.
2. **CLI**: We can create an application using the Argo CD CLI.
3. **Git Repository**: We can define the application in a Git repository and Argo CD will automatically create the application. It is a Declarative way of defining the application. Most Recommended way.

### Declarative Way of Defining Application

We can use the below YAML file to define an application in Argo CD:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: guestbook
    repoURL: https://github.com/argoproj/argocd-example-apps.git
    targetRevision: HEAD
  syncPolicy:
    automated:
      prune: true
      selfHeal: true  
```

### Using CLI to Create an Application

We can use the below command to create an application using the Argo CD CLI:

```bash
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --revision HEAD --dest-server https://kubernetes.default.svc --dest-namespace default
```

### Using Web UI to Create an Application

![Screenshot 2024-04-26 230311](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/b55ef6e1-8444-4eea-a0e8-c6aee863a82c)

![Screenshot 2024-04-26 230516](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/42112b43-1d1b-4925-988b-2c7e1005fcc2)

![Screenshot 2024-04-26 230559](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/a7b5325f-219b-496c-a7b3-bcc066d844fb)

![Screenshot 2024-04-26 230624](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/eaeb387a-e646-4ffa-8c8f-49b2de5a2f2c)

![Screenshot 2024-04-26 230633](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/11f60d0d-785f-4603-855e-3f90ad7cd1b6)

![Screenshot 2024-04-26 231931](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/5362f076-b873-41c9-8650-f4d7f7e24b34)

![Screenshot 2024-04-26 231950](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/649c851b-1190-40d4-bc24-dd090cdefa5e)


Date of Creation: 26/04/2024