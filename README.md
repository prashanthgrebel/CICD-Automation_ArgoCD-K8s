# ArgoCD CLI Commands

# * List apps
  ``` argocd list apps ```
# * create app 
  ``` argocd app create test-1 --repo https://github.com/prashanthgrebel/CICD-Automation_ArgoCD-K8s.git --path spring-boot-app --dest-namespace default --dest-server https://kubernetes.default.svc --directory-recurse ```
# * sync app
  ``` argocd app sync argocd/test-1 ```
# * lsit projects list
``` argocd proj list```

# argocd kubenetes commands
# - get application status
```
root@kmaster1-115:~# kubectl get applications -n argocd
NAME     SYNC STATUS   HEALTH STATUS
test-1   Synced        Healthy
root@kmaster1-115:~# 
root@kmaster1-115:~#
```
# - get project status
```
root@kmaster1-115:~# kubectl get appproj -n argocd
NAME          AGE
default       38d
spring-boot   38d
```
# - get projec details
```
# argocd proj get <project name>

root@kmaster1-115:~# argocd proj get spring-boot
Name:                        spring-boot
Description:                 spring-boot app
Destinations:                <none>
Repositories:                https://github.com/prashanthgrebel/CICD-Automation_ArgoCD-K8s.git
Scoped Repositories:         <none>
Allowed Cluster Resources:   <none>
Scoped Clusters:             <none>
Denied Namespaced Resources: <none>
Signature keys:              <none>
```
