# ArgoCD CLI Commands

* List apps
  ``` argocd list apps ```
* create app
  ``` argocd app create test-1 --repo https://github.com/prashanthgrebel/CICD-Automation_ArgoCD-K8s.git --path spring-boot-app --dest-namespace default --dest-server https://kubernetes.default.svc --directory-recurse ```
* sync app
  ``` argocd app sync argocd/test-1 ```
