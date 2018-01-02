# bikeride-cicd-kubernetes
CI / CD toolset for Bikeride deployment on Kubernetes using helm



## Deploying NGINX ingress
https://github.com/kubernetes/charts/tree/master/stable/jenkins

```
helm install --name loadbalancer -f loadbalancer-values.yml stable/nginx-ingress
```

```
helm del --purge loadbalancer
```

## Deploying GitLab
https://github.com/kubernetes/charts/tree/master/stable/gitlab-ce

```
kubectl create -f gitlab-gluster.yml
helm install --name gitlab-ce -f gitlab-values.yml stable/gitlab-ce
```

To revert the installation:
```
helm del --purge gitlab-ce
kubectl delete -f gitlab-gluster.yml
kubectl get all
```

## Deploying Jenkins
https://github.com/kubernetes/charts/tree/master/stable/jenkins

```
kubectl create -f jenkins-gluster.yml
helm install --name jenkins -f jenkins-master-values.yml stable/jenkins
```

To revert the installation:
```
helm del --purge jenkins
kubectl delete -f jenkins-gluster.yml
kubectl get all
```


## Deploying Artifactory
https://github.com/kubernetes/charts/tree/master/stable/artifactory

```
kubectl create -f artifactory-gluster.yml
helm install --name artifactory -f artifactory-values.yml stable/artifactory
```

By default the username is **admin/password**

To revert the installation:
```
helm del --purge artifactory
kubectl delete -f artifactory-gluster.yml
kubectl get all
```
## Deploying SonarQube
https://github.com/kubernetes/charts/tree/master/stable/sonarqube

```
kubectl create -f sonarqube-gluster.yml
helm install --name sonarqube -f sonarqube-values.yml stable/sonarqube
```

To revert the installation:
```
helm del --purge sonarqube
kubectl delete -f sonarqube-gluster.yml
kubectl get all
```

## Deploying Docker registry


