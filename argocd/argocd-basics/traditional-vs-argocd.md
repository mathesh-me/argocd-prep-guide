## Traditional CD vs Argo CD

### Traditional CD

In Traditional CD, the Deployment part is also done by the CI tool itself. The CI tool will build the application, run the tests, and deploy the application to the target environment. The CI tool will have the logic to deploy the application to the target environment.

`Build -> Test -> Build Image -> Push Image into Registry -> Update image tag in K8S Manifest -> Push Manifest into Git Repository -> Deploy using kubectl`

#### Disadvantages of Traditional CD

- We have to install `kubectl` and `helm` in the CI/CD server.
- We have to grant access to the CI/CD server to Update the Kubernetes Cluster.

### Argo CD

In Argo CD, the Deployment part is done by the Argo CD tool. Argo CD will continuously monitor the desired application state defined in the Git repository and compare it with the live state of the application running on the Kubernetes cluster. If there is any drift between the desired state and the live state, Argo CD will automatically reconcile the drift. The CI part is done by the CI tool itself. The CI tool will build the application, run the tests, and push the artifacts to the Git Repository. Argo CD will take care of the deployment part.

`Build -> Test -> Push Image into Registry -> Update image tag in K8S Manifest -> Push Manifest into Git Repository -> Deploy using Argo CD`

Date of Creation: 26/04/2024