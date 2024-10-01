apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: four-lost-app
spec:
  destination:
    name: ''
    namespace: default
    server: https://kubernetes.default.svc
  source:
    path: helm-guestbook
    repoURL: https://github.com/argoproj/argocd-example-apps/
    targetRevision: HEAD
  sources: []
  project: default