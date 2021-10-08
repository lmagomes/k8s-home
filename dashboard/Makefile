# From https://rancher.com/docs/k3s/latest/en/installation/kube-dashboard/

export GITHUB_URL=https://github.com/kubernetes/dashboard/releases
export VERSION_KUBE_DASHBOARD=$(shell curl -w '%{url_effective}' -I -L -s -S ${GITHUB_URL}/latest -o /dev/null | sed -e 's|.*/||')

.PHONY: delete-dashboard
delete-dashboard:
	k3s kubectl delete ns kubernetes-dashboard

.PHONY: setup-dashboard
setup-dashboard:
	k3s kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/${VERSION_KUBE_DASHBOARD}/aio/deploy/recommended.yaml

.PHONY: update-dashboard
update-dashboard: delete-dashboard
update-dashboard:
	k3s kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/${VERSION_KUBE_DASHBOARD}/aio/deploy/recommended.yaml -f dashboard.admin-user.yml -f dashboard.admin-user-role.yml

setup-rbac:
	k3s kubectl create -f dashboard.admin-user.yml -f dashboard.admin-user-role.yml

.PHONY: display-token
display-token:
	k3s kubectl -n kubernetes-dashboard describe secret admin-user-token | grep '^token'

.PHONY: start-proxy
start-proxy:
start-proxy: display-token
	@echo
	@echo Access http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
	@echo
	k3s kubectl proxy
