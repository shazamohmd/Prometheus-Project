# Monitoring Kubernetes Cluster with Prometheus

Monitoring a Kubernetes cluster with Prometheus and Grafana on CentOS 9 on the same machine involves setting up both Prometheus and Grafana services and configuring them to collect and visualize metrics from the Kubernetes cluster running on the same node going through some steps including installation of the following: Docker, Kubectl, Minikube, Helm, Prometheus and Grafana.

## Install Docker 
1. Set up the repository
```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
 2. Install Docker 
```bash
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
```
3. Start and enable Docker 
```bash
sudo systemctl enable docker
sudo systemctl start docker
```
## Install kubectl
1. Download the latest release 
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
2. Download the kubectl checksum file
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```
3. Install kubectl
```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
## Install Minikube
1. Install Kubelet 
```bash
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubelet
```
2. Download the latest version of Minikube
```bash
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
3. Make the binary executable
```bash
chmod +x minikube-linux-amd64
```
4. Move the binary to the /usr/local/bin
```bash
 sudo mv minikube-linux-amd64 /usr/local/bin/minikube
```
5. Start Minikube 
```bash
minikube start 
```
![prom1](https://github.com/shazamohmd/Prometheus-Project-/assets/161209672/b4d73097-399a-4873-afc6-ed673c691891)



## Install Helm 
1. Download Helm installation script
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```
2. Provide execute permission 
```bash
chmod 700 get_helm.sh
```
3. Run the Helm installation script
```bash
./get_helm.sh
```
## Install Prometheus 
1. Add Prometheus repository 

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
2. Install Prometheus using Helm

```bash
helm install prometheus prometheus-community/prometheus
```
![prom2](https://github.com/shazamohmd/Prometheus-Project-/assets/161209672/79388ae0-a1d4-45cc-9509-f2892bb2c061)

3. Expose Prometheus service
```bash
kubectl expose service prometheus-server -- type=NodePort --target-port=9090 -- name=prometheus-server-ext
```
![prom5](https://github.com/shazamohmd/Prometheus-Project-/assets/161209672/361775dc-2910-44de-adc2-7b3b1008fd62)

## Install Grafana 

1. Add Grafana repository
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
2. Install Grafana using Helm
```bash
helm install grafana grafana/grafana
```
3. Expose Grafana service
```bash
kubectl expose service grafana -- type=NodePort -- target-port=3000 -- name=grafana-ext
```
4. Get Grafana Credintials
```bash
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

## Conclusion 

In conclusion, this project installed and configured key tools like Docker, Kubectl, Minikube, Helm, Prometheus, and Grafana to enable effective monitoring of our Kubernetes environment. By utilizing Prometheus and Grafana, we were able to collect, store, and visualize metrics from our Kubernetes cluster, gaining valuable insights into its performance, resource utilization, and overall health. This setup allows us to monitor and troubleshoot issues promptly, ensuring the dependability and stability of our Kubernetes workloads.
![prom7](https://github.com/shazamohmd/Prometheus-Project-/assets/161209672/a2ee6d5c-15b5-4294-b565-bf58236c1d03)
![g1](https://github.com/shazamohmd/Prometheus-Project-/assets/161209672/5bcf5c97-62c1-47cc-9e19-929b628ed095)


)
