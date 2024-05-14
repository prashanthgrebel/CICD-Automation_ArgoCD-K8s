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
