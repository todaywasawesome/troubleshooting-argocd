This example uses a job to automatically scale up a deployment down to 0 replicas which then causes the application to show out of sync. If autosync with self heal is enabled this will cause the application to constantly sync. This is an example of flapping.

To fix this we can edit the application to ignore the replicas difference.

```yaml
ignoreDifferences:
- group: apps
  kind: Deployment
  jsonPointers:
  - /spec/replicas
```

Read more: https://argo-cd.readthedocs.io/en/stable/user-guide/diffing/#ignore-differences