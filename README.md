# bikeride-cicd-kubernetes
CI / CD toolset for Bikeride deployment on Kubernetes using helm


## Deploying GitLab
https://github.com/kubernetes/charts/tree/master/stable/gitlab-ce

```
kubectl create -f storageclass.yml
helm install --name gitlab-ce -f values.yaml stable/gitlab-ce
```

```
helm del --purge gitlab-ce
kubectl delete -f storageclass.yml
kubectl get all
```

## Deploying Nexus


## Deploying Jenkins
https://github.com/kubernetes/charts/tree/master/stable/jenkins


## Deploying Docker registry