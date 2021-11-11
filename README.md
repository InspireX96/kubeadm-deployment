# kubeadm-deployment
Deploy on kubeadm cluster

## Argo CD

### Installation

Argo CD needs to be installed first, then it will help to manage deployments in this repo

[https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)

```
# install Argo CD
cd argocd/overlays/dev
make install


# expose Argo CD by NodePort and get login password
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Initialize project and environments so ArgoCD can self-manage from now on
