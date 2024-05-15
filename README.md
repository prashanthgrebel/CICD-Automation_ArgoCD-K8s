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
# argocd proj get spring-boot -o yaml -- form yaml format

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
# RECONCILIATION
* 1. RECONCILIATION TIMEOUT
* 2. RECONCILIATION WEBHOOK

# - check ARGOCD_RECONCILIATION_TIMEOUT
```
root@kmaster1-115:~# kubectl describe po argocd-repo-server -n argocd  | grep -i "ARGOCD_RECONCILIATION_TIMEOUT" -B1
    Environment:
      ARGOCD_RECONCILIATION_TIMEOUT:                                <set to the key 'timeout.reconciliation' of config map 'argocd-cm'>                                          Optional: true
```
# - patch /modify ARGOCD_RECONCILIATION_TIMEOUT
```
root@kmaster1-115:~# kubectl patch configmap argocd-cm --patch='{"data":{"timeout.reconciliation":"60s"}}' -n argocd 
configmap/argocd-cm patched

root@kmaster1-115:~# kubectl rollout restart deploy argocd-repo-server -n argocd
deployment.apps/argocd-repo-server restarted
root@kmaster1-115:~# 
```
# - RECONCILIATION WEBHOOK



