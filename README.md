# k8s
Prometheus
Install using helm chart with as much customization as possibly you know, then you can configure additional settings using crds whose yaml manifests stay in git. The problem with this is when you do helm delete

Components:
kube-state-metrics
exposes k8s objects metrics:
pods, svc, deployment, configmaps, pv, pvc, node status

Accessible via: IP:8080/metrics # Use port-forward to access
Alerts to create:
Check if specific pods/deployements are ready, use AI to create the query


Node_exporter:
IP:9100 # Use port-forward to access
