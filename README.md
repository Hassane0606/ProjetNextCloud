# Kubernetes Project Setup

## Create Cluster & Set Kubeconfig

```sh
eksctl create cluster -f cluster.yml
aws eks update-kubeconfig --name projet-cluster --region us-east-1
