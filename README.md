EFK (Elasticsearch, Fluentd, Kibana) Stack Deployment on Kubernetes
This README explains how to deploy the EFK stack on a Kubernetes cluster. It will help you set up Elasticsearch, Fluentd, and Kibana for collecting, processing, and visualizing logs.


Step 1: Deploy Elasticsearch
Elasticsearch is used to store the logs collected from Fluentd.
# Elasticsearch StatefulSet
Command to apply:
kubectl apply -f elasticsearchstatefulset.yaml
# Elasticsearch Service
apiVersion: v1
kind: Service
metadata:
  name: elasticsearch
  namespace: kube-logging
  labels:
    app: elasticsearch
spec:
  selector:
    app: elasticsearch
  clusterIP: None
  ports:
    - port: 9200
      name: rest
    - port: 9300
      name: inter-node

Command to apply:
kubectl apply -f elasticsearchservice.yaml

Step 2: Deploy Fluentd
Fluentd collects logs from Kubernetes containers and forwards them to Elasticsearch.

2.1: Create Fluentd ConfigMap
# Fluentd ConfigMap
Command to apply:
kubectl apply -f fluentd-configmap.yaml

2.2: Create Fluentd RBAC
# Fluentd RBAC configuration
Command to apply:
kubectl apply -f fluentd-RBAC.yaml

2.3: Deploy Fluentd DaemonSet
# Fluentd DaemonSet
Command to apply:
kubectl apply -f fluentd-daemonset.yaml



Step 3: Deploy Kibana
Kibana is used to visualize the logs stored in Elasticsearch.

# Kibana Deployment
Command to apply:
kubectl apply -f kibana-deployment.yaml

---
# Kibana Service
Command to apply:
kubectl apply -f kibana-service.yaml


Step 4: Access Kibana
After deploying Kibana, you can access it via the NodePort. Use kubectl get svc -n kube-logging to get the external IP or NodePort.

Open the Kibana UI in your browser, and configure an index pattern to start visualizing logs.



