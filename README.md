# 🐳 Kubernetes Deployment with Prometheus & Grafana Monitoring on EKS

This project demonstrates deploying a containerized Apache-PHP web application on **Amazon EKS**, and monitoring it using **Prometheus** and **Grafana** via Helm. It also features **Horizontal Pod Autoscaling (HPA)** based on CPU usage.

---

## 📦 Project Structure

```bash
.
├── mywebd-deployment.yaml         # Deployment manifest for Apache-PHP web server
└── README.md                      # Project documentation

🚀 What I Did
✅ 1. Deployed a Web Application on EKS
Used kubectl to create a deployment:
kubectl create deployment mywebd --image=vimal13/apache-webserver-php --dry-run=client -o yaml > mywebd-deployment.yaml

Applied the YAML to EKS:
kubectl apply -f mywebd-deployment.yaml

🌐 2. Exposed the App via LoadBalancer
Exposed deployment over port 80 using:
kubectl expose deployment mywebd --type=LoadBalancer --port=80

📊 3. Installed Prometheus & Grafana Using Helm
Added the kube-prometheus-stack repo:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

Installed monitoring stack:
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace

🔒 4. Accessed Grafana Dashboard
Edited Grafana service to LoadBalancer:
kubectl edit svc monitoring-grafana -n monitoring

Fetched external URL:
kubectl get svc -n monitoring

Default credentials:
Username: admin
Password: prom-operator (can vary, check with: kubectl get secret)

⚙️ 5. Configured Horizontal Pod Autoscaler (HPA)
Enabled autoscaling based on 50% CPU:
kubectl autoscale deployment mywebd --cpu-percent=50 --min=1 --max=10

Simulated traffic using busybox:
kubectl run -i --tty load-generator --image=busybox /bin/sh
while true; do wget -q -O- http://<EXTERNAL-IP>; done

📈 6. Observed Scaling and Metrics in Grafana
Verified HPA:
kubectl get hpa

Watched pods scale in/out depending on traffic.

Imported Kubernetes dashboards in Grafana to monitor everything.

🌍 Live Monitoring
Application URL: http://<LoadBalancer-IP>

Grafana URL: http://<Grafana-LoadBalancer-IP>

Grafana Dashboards: Kubernetes / Node Exporter / Prometheus metrics



