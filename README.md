# k8s
## charmuseum
### Install and run
```
chartmuseum --port=<port> --storage="local" --storage-local-rootdir="<chart-dir>"
```
### Add repo and update
```
helm repo add chartmuseum http://localhost:8080
helm repo update
```
### Download chart directly
```wget  http://localhost:8081/charts/mysql-1.0.3.tg```

### Package and upload
```
git clone https://github.com/stakater/chart-mysql.git
cd chart-mysql/mysql
helm package .
ls
curl -v -L --data-binary "@mysql-1.0.3.tgz" http://localhost:8081/api/charts

helm repo update
helm install chartmuseum/mysql
```

## Prometheus
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


## Grafana:
### Adding new dashboard to values.yaml
```
+dashboards:
+  default:
+    node-exporter:
+      json: |
+        {
```
### Add datasource to values.yaml
```
+datasources:
+  datasources.yaml:
+    apiVersion: 1
+    datasources:
+    - name: Prometheus
+      type: prometheus
+      url: http://prometheus-server.monitoring.svc.cluster.local
+      access: proxy
+      isDefault: true
```
### Add provider to values.yaml
```
+dashboardProviders:
+  dashboardproviders.yaml:
+    apiVersion: 1
+    providers:
+    - name: 'default'
+      orgId: 1
+      folder: ''
+      type: file
+      disableDeletion: false
+      editable: true
+      options:
+        path: /var/lib/grafana/dashboards/default

kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
export POD_NAME=$(kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}")
     kubectl --namespace monitoring port-forward $POD_NAME 3000
```  
## ingress
```
 ingress:
-  enabled: false
-  # For Kubernetes >= 1.18 you should specify the ingress-controller via the field ingressClassName
-  # See https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/#specifying-the-class-of-an-ingress
-  # ingressClassName: nginx
-  # Values can be templated
-  annotations: {}
-    # kubernetes.io/ingress.class: nginx
-    # kubernetes.io/tls-acme: "true"
-  labels: {}
+  enabled: true
+  ingressClassName: nginx
+  annotations:
+    kubernetes.io/ingress.class: nginx
+    nginx.ingress.kubernetes.io/ssl-redirect: "false"

   hosts:
-    - chart-example.local
+    - grafana.local
```

