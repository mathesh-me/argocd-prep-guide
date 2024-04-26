## Hands-On using Argo CD

### Installl Argo CD using `kubectl`

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Expose the Argo CD Server

- Edit the `argocd-server` service to change the `type` to `NodePort`.
```bash
kubectl edit svc argocd-server -n argocd
```
- Change the `type` to `NodePort` and save the file.
- We have to do some `port-forwarding` to access the Argo CD UI.
- In my Case, I used `minikube` to set up a Kubernetes Cluster. So I am going to install `socat` to do port-forwarding.
```bash
sudo apt-get install socat
```
- Run the following command to do port-forwarding.
```bash
socat TCP4-LISTEN:8080,fork,reuseaddr TCP4:192.168.49.2:NodePort &
```
- Now we can access the Argo CD UI using `http://NodeIP:8080`.
- To get the password for the `admin` user.
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
- Now we can login to the Argo CD UI using the `admin` user and the password we got from the above command.

### Create an Application using Argo CD

- Click on the `New App` button.
- Fill in the details like `Application Name`, `Project`, `Repository URL`, `Path`, `Cluster URL`, `Namespace`, `Sync Policy`, `Sync Options`, etc.
- Click on the `Create` button.

Used Repository URL: `https://github.com/argoproj/argocd-example-apps`

### Results:

![Screenshot 2024-04-17 073904](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/8b6c3db2-c946-45cb-b864-97ddfcc482ff)

![Screenshot 2024-04-17 073843](https://github.com/mathesh-me/argo-cd-prep/assets/144098846/d739a586-4fe1-41d7-a885-742422b7a018)


