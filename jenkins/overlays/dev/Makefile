helm_name = jenkins
namespace = jenkins
helm_file = jenkins-values.yaml
chart = jenkinsci/jenkins

.PHONY: add-repo
add-repo:
	helm repo add jenkinsci https://charts.jenkins.io
	helm repo update

.PHONY: kustomize
kustomize:
	kubectl apply -k ./

.PHONY: install
install: kustomize
	helm install $(helm_name) -n $(namespace) -f $(helm_file) $(chart)

.PHONY: upgrade
upgrade: kustomize
	helm upgrade $(helm_name) -n $(namespace) -f $(helm_file) $(chart)

.PHONY: uninstall
uninstall:
	helm uninstall $(helm_name) -n $(namespace)
	kubectl delete -k ./
