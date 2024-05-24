# Kubernetes
** Create cluster & set kubeconfig


** Create the namespace

# Nextcloud
** Adding the helm repo for nextcloud
helm repo add nextcloud https://nextcloud.github.io/helm/

** Installing & Updating


** Connect to nextcloud


** Get credentials


# Helm
'''List all helm releases'''


'''Uninstall a helm release'''

# Flux CD
** Install Flux
export GITHUB_TOKEN="<token>"
flux bootstrap github --owner=myahia93 --repository=FluxCD_Nextcloud_EKS --path=clusters/project --personal --private=false

** Deploying the UI
flux create source helm ww-gitops \
 --url=https://helm.gitops.weave.works \
 --export > ./clusters/project/weave-gitops-source.yml

flux create helmrelease ww-gitops \
 --source=HelmRepository/ww-gitops \
 --chart=weave-gitops \
 --values=./weave-gitops-values.yml \
 --export > ./clusters/project/weave-gitops-helmrelease.yaml

** Access to the UI
kubectl port-forward pod/ww-gitops-weave-gitops-6fb6f5fb57-skcmn 9001:9001 -n flux-system
# Nextcloud Using Flux
** Create the Helm source
flux create source helm nextcloud \
  --url=https://nextcloud.github.io/helm/ \
  --export > ./clusters/project/nextcloud-source.yaml

** Create the HelmRelease
flux create helmrelease nextcloud \
  --source=HelmRepository/nextcloud \
  --chart=nextcloud \
  --target-namespace=nextcloud \
  --values=./nextcloud-values.yaml \
  --interval=1m \
  --export > ./clusters/project/nextcloud-helmrelease.yaml

** Connect to Nextcloud ** 
k port-forward svc/nextcloud-nextcloud 8080:8080
