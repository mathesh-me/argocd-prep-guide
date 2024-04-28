## Accessing Argo CD Web UI

By Default, the Argo CD Web UI is not exposed outside the cluster. To access the Argo CD Web UI, we have to port-forward the Argo CD Server service to our local machine.

### How to Access Argo CD Web UI?

- We have to edit the Argo CD Server service to expose the service outside the cluster.
- We can find the Argo CD Server service in the `argocd` namespace.
- We have to change the `type` field from `ClusterIP` to `LoadBalancer` or `NodePort`. Also we can use ingress and Port-forwarding.
- Once we change the service type, we can access the Argo CD Web UI using the service IP and port.
- Now we can access the Argo CD Web UI using the below URL:

```bash
http://<node-ip>:<service-port>
```

Date of Notes: 27/04/2024