# Monitoring-and-Logging-
1)Run the following command to install Chocolatey, this chocolaty is useful for intalling ekctl, kubectl commandline on windows.
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

2)Then configure aws access by hitting below command
aws configure and provide the relavant access and secret access keys

3)Create eks cluster 
eksctl create cluster --name 
observabilityandlogging --region us-west-1 --nodegroup-name standard-workers --node-type t2.medium --nodes 3 --nodes-min 1 --nodes-max 4 --managed

 ![image](https://github.com/user-attachments/assets/fb6c763c-9625-4d75-b717-37e2e6c265fe)

4)Once Chocolatey is working, you can install Helm with:Helm is the most straightforward way to deploy applications on Kubernetes, including Prometheus and Grafana.
choco install kubernetes-helm

5)Add the Prometheus community charts to Helm:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

6)Now, install Prometheus on your EKS cluster using Helm. You can customize the Helm chart options, but here is the basic command
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
This command will Install Prometheus in the monitoring namespace. Install the Prometheus Operator, Alertmanager, Grafana, and kube-state-metrics.

7)To check the pods deployed in the monitoring namespace:
kubectl get pods -n monitoring

8)The Grafana instance is automatically installed as part of the Prometheus Helm chart. To access the Grafana dashboard:
Port-forward the Grafana service to your local machine:
kubectl port-forward -n monitoring svc/prometheus-grafana 3000:80
![image](https://github.com/user-attachments/assets/0d9df730-31a4-4428-a9d8-ab558baf2cee)![image](https://github.com/user-attachments/assets/a2e0b0eb-edf7-4248-8044-a8c334455c99)
9)Install Fluent Bit Using Helm
Fluent Bit helps collect logs from your EKS nodes and sends them to CloudWatch. Use Helm to deploy Fluent Bit in your EKS cluster.
helm repo add fluent https://fluent.github.io/helm-charts
helm repo update
10)Install Fluent Bit in your logging namespace:
helm install fluent-bit fluent/fluent-bit --namespace logging --create-namespace \
  --set backend.type=cloudwatch \
  --set backend.cloudwatch.region=us-west-2 \
  --set backend.cloudwatch.logGroupName=/aws/eks/cluster-logs \
  --set backend.cloudwatch.logStreamPrefix=eks-

10)Verify Fluent Bit Installation
kubectl get pods -n logging

11)Check Logs in CloudWatch
To view the logs in AWS CloudWatch:
Go to the AWS CloudWatch console.



