# bikeride-cicd-kubernetes
CI / CD toolset for Bikeride deployment on Kubernetes using helm


## Configure the storage class to point to the internal Heketi
```
export HEKETI_URL=http://`kubectl get services|grep heketi-cluster|awk '{print $3}'`:`kubectl get service |grep heketi-cluster |awk '{print $5}' |awk '{split($0,a,"/"); print a[1]}'`
echo $HEKETI_URL
```

Adjust the `local-gluster.yml` file with the URL result of the commands above and the password provided when configuring Heketi
  * `resturl: "http://10.110.90.248:8080"`
  * `restuserkey: "Welcome1"`
  * `volumetype: "replicate:2"`

and then create the storage class with the following command:

```
kubectl create -f local-gluster.yml
```

## Deploying NGINX ingress
https://github.com/kubernetes/charts/tree/master/stable/jenkins

```
helm install --name loadbalancer -f loadbalancer/loadbalancer-values.yml stable/nginx-ingress
```

To revert the installation:
```
helm del --purge loadbalancer
```

## Deploying GitLab
https://github.com/kubernetes/charts/tree/master/stable/gitlab-ce

```
helm install --name gitlab-ce -f gitlab-ce/gitlab-values.yml stable/gitlab-ce
```

To revert the installation:
```
helm del --purge gitlab-ce
```

## Deploying Jenkins
https://github.com/kubernetes/charts/tree/master/stable/jenkins

https://medium.com/@thepauleh/scalable-jenkins-with-helm-and-kubernetes-207df883ac9a

```
helm install --name jenkins -f jenkins/jenkins-values.yml stable/jenkins
```
admin / Welcome1

To configure the agents
* Manage Jenkins -> Configure System
    * Cloud
        * Name: kubernetes
        * Kubernetes URL: https://kubernetes.default.svc.cluster.local
        * Disable https certificate check: checked
        * Jenkins URL: http://192.168.40.81:8080
        * Jenkins tunnel: 10.97.148.144:50000 (jenkins-jenkins-agent service)
        * Kubernetes Pod Template
            * Name: jenkins-slave
            * Labels: jenkins-slave
            * EnvVars:
                * Key: JENKINS_URL
                * Vakue: http://192.168.40.81:8080

Support to pipelines: https://github.com/jenkinsci/kubernetes-plugin/blob/master/README.md
Support to DinD slaves: 
    * https://hub.docker.com/r/billyteves/jenkinslave-dind-kubernetes/
    * 
                
To revert the installation:
```
helm del --purge jenkins
```

## Deploying Artifactory
https://github.com/kubernetes/charts/tree/master/stable/artifactory

```
helm install --name artifactory -f artifactory/artifactory-values.yml stable/artifactory
```

By default the username is **admin/password**

To revert the installation:
```
helm del --purge artifactory
```
## Deploying SonarQube
https://github.com/kubernetes/charts/tree/master/stable/sonarqube

```
helm install --name sonarqube -f sonarqube/sonarqube-values.yml stable/sonarqube
```

To revert the installation:
```
helm del --purge sonarqube
```

## Deploying Docker registry
https://github.com/kubernetes/charts/tree/master/stable/docker-registry
```
helm install --name docker-registry -f docker-registry/docker-registry-values.yml stable/docker-registry
```

To revert the installation:
```
helm del --purge docker-registry
```
