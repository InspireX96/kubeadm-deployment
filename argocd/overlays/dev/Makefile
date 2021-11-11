namespace = argocd

.PHONY: kustomize
kustomize:
	kubectl apply -k .


.PHONY: install
install: kustomize
	kubectl apply -n $(namespace) -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

.PHONY: uninstall
uninstall:
	kubectl delete -n $(namespace) -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
	kubectl delete -k ./
