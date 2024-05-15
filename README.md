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

![image](https://github.com/prashanthgrebel/CICD-Automation_ArgoCD-K8s/assets/92351464/75f708d4-276f-41d0-b843-8249018a2ea9)


# RBAC

* 1. add users to argocd-cm as below
```
data:
  accounts.pavi: login, apikey
  accounts.rebel: login, apikey
```


```
apiVersion: v1
data:
  accounts.pavi: login, apikey
  accounts.rebel: login, apikey
  timeout.reconciliation: 60s
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","data":{"accounts.rebel":"login","policy.cvs":"p, role:rebel, applications, sync, */*, allow\n","timeout.reconciliation":"60s"},"kind":"ConfigMap","metadata":{"annotations":{},"creationTimestamp":"2024-04-05T13:41:11Z","labels":{"app.kubernetes.io/name":"argocd-cm","app.kubernetes.io/part-of":"argocd"},"name":"argocd-cm","namespace":"argocd","resourceVersion":"1040211","uid":"3018c518-b128-488e-a444-5682d6cd861b"}}
  creationTimestamp: "2024-04-05T13:41:11Z"
  labels:
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
  name: argocd-cm
  namespace: argocd
  resourceVersion: "1063914"
  uid: 3018c518-b128-488e-a444-5682d6cd861b
```
* 2. Create RBAC policy in argocd-rbac-cm
```
data:
  policy.csv: |
    p, role:org-admin, applications, *, default/*, allow
    g, rebel, role:org-admin
```
```
apiVersion: v1
data:
  policy.csv: |
    p, role:org-admin, applications, *, default/*, allow
    g, rebel, role:org-admin
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","data":{"policy.csv":"p, role:org-admin, applications, *, *, allow\ng, pavi, role:org-admin\n"},"kind":"ConfigMap","metadata":{"annotations":{},"creationTimestamp":"2024-04-05T13:41:11Z","labels":{"app.kubernetes.io/name":"argocd-rbac-cm","app.kubernetes.io/part-of":"argocd"},"name":"argocd-rbac-cm","namespace":"argocd","resourceVersion":"755313","uid":"dd0441a9-7b22-4b2b-a383-a8d850d91a0c"}}
  creationTimestamp: "2024-04-05T13:41:11Z"
  labels:
    app.kubernetes.io/name: argocd-rbac-cm
    app.kubernetes.io/part-of: argocd
  name: argocd-rbac-cm
  namespace: argocd
  resourceVersion: "1052797"
  uid: dd0441a9-7b22-4b2b-a383-a8d850d91a0c
```

* Refer : https://argo-cd.readthedocs.io/en/stable/operator-manual/argocd-rbac-cm-yaml/
* Refer : https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/


