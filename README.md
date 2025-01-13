# k8s
Prometheus
Install using helm chart, then configure additional settings using crds whose yaml manifests stay in git.

Components:
kube-state-metrics
exposes k8s objects metrics:
pods, svc, deployment, configmaps, pv, pvc, node status

Accessible via: IP:8080/metrics
Alerts to create:
Check if specific pods/deployements are ready, use AI to create the query
