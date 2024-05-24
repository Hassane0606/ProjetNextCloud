# Kubernetes
'''Create cluster & set kubeconfig'''

# Nextcloud
'''Adding the helm repo for nextcloud'''
helm repo add nextcloud https://nextcloud.github.io/helm/

'''Installing & Updating'''

'''Connect to nextcloud'''

'''Get credentials'''


# Helm
'''List all helm releases'''


'''Uninstall a helm release'''

# Flux CD
'''Install Flux'''


'''Deploying the UI'''
flux create source helm ww-gitops \
 --url=https://helm.gitops.weave.works \
 --export > ./clusters/project/weave-gitops-source.yml

flux create helmrelease ww-gitops \
 --source=HelmRepository/ww-gitops \
 --chart=weave-gitops \
 --values=./weave-gitops-values.yml \
 --export > ./clusters/project/weave-gitops-helmrelease.yaml

'''Access to the UI'''
kubectl port-forward pod/ww-gitops-weave-gitops-6fc66d8597-52w65 9001:9001 -n flux-system

# Nextcloud Using Flux
'''Create the Helm source'''
flux create source helm nextcloud \
  --url=https://nextcloud.github.io/helm/ \
  --export > ./clusters/project/nextcloud-source.yaml

'''Create the HelmRelease'''
flux create helmrelease nextcloud \
  --source=HelmRepository/nextcloud \
  --chart=nextcloud \
  --target-namespace=nextcloud \
  --values=./nextcloud-values.yaml \
  --interval=1m \
  --export > ./clusters/project/nextcloud-helmrelease.yaml

'''Connect to Nextcloud '''
kubectl port-forward pod/nextcloud-nextcloud-65c4bd89f4-t44ql 8080:80 -n nextcloud
