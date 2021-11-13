# kubeadm-deployment
Deploy on kubeadm cluster

## Argo CD

Continuous delivery tool for k8s

### Installation

Argo CD needs to be installed first, then it will help to manage deployments in this repo

[https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)

```
# install Argo CD
cd argocd/base/kustomize
make install


# expose Argo CD by NodePort and get login password
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort", "ports": [{"port":80, "nodePort": 30864}]}}'
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Initialize project and environments

```
cd environments
make kustomize 
```

ArgoCD can self-manage from now on

## Kube-prometheus stack

Managed by Argo CD, for monitoring purpose

### Grafana

The default username and password is `admin/prom-operator`
