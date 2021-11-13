# kubeadm-deployment
Deploy on kubeadm cluster

## Argo CD

Continuous delivery tool for k8s

### Installation

Argo CD needs to be installed first, then it will help to manage deployments in this repo

[https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)

```bash
# install Argo CD
cd argocd/base/kustomize
make install


# expose Argo CD by NodePort and get login password
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort", "ports": [{"port":80, "nodePort": 30864}]}}'
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Initialize project and environments

```bash
cd environments
make kustomize 
```

Argo CD can self-manage from now on

### Add environment

To add a new environment, add a project in `environments/project.yaml` and add an app in `environments/apps.yaml`. Create a subdirectory under `environments/` and add the application yaml files. Then commit and push to master branch

```bash
cd argocd/base/kustomize
make instal
```

New project will be synced by Argo CD

## Kube-prometheus stack

Managed by Argo CD, for monitoring purpose

### Grafana

The default username and password is `admin/prom-operator`

## Jenkins

Deploy the legendary automation server in k8s using helm

### Installation

Jenkins is not managed by Argo CD due to its deployment complexity. It will require manual installation/upgrade

[https://www.jenkins.io/doc/book/installing/kubernetes/#install-jenkins-with-helm-v3](https://www.jenkins.io/doc/book/installing/kubernetes/#install-jenkins-with-helm-v3)


```bash
cd jenkins/overlays/dev

make add-repo
make install
```

### Upgrade

After upgrading the helm chart `jenkins/overlays/dev/jenkins-values.yaml`


```bash
cd jenkins/overlays/dev

make upgrade
```

### NOTES

```
NOTES:
1. Get your 'admin' user password by running:
  kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/chart-admin-password && echo
2. Get the Jenkins URL to visit by running these commands in the same shell:
  export NODE_PORT=$(kubectl get --namespace jenkins -o jsonpath="{.spec.ports[0].nodePort}" services jenkins)
  export NODE_IP=$(kubectl get nodes --namespace jenkins -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT/login

3. Login with the password from step 1 and the username: admin
4. Configure security realm and authorization strategy
5. Use Jenkins Configuration as Code by specifying configScripts in your values.yaml file, see documentation: http:///configuration-as-code and examples: https://github.com/jenkinsci/configuration-as-code-plugin/tree/master/demos

For more information on running Jenkins on Kubernetes, visit:
https://cloud.google.com/solutions/jenkins-on-container-engine

For more information about Jenkins Configuration as Code, visit:
https://jenkins.io/projects/jcasc/


NOTE: Consider using a custom image with pre-installed plugins
```
