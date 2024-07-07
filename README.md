**# Installing Helm package manager:**
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

**#Adding Falco helm charts to helm repos:**
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update

**#Creating custom_rules.yaml file:**
Refer to the file.

**#Installing Falco on AKs using default Charts.yaml and Values.yaml file**:
helm install falco -f custom_rules.yaml falcosecurity/falco \
    --version 4.0.0 \
    --create-namespace \
    --namespace falco \
    --set driver.kind=ebpf \
    --set tty=true \
    --set falcosidekick.enabled=true \
    --set falcosidekick.webui.enabled=true \
    --set falcosidekick.webui.user=<username>:<password> \
    --set falcosidekick.webui.service.type=LoadBalancer \
    --set falco.json_output=true \
    --set falco.log_level=info \
    --set collectors.kubernetes.enabled=true

**#Installing Event-generator**
helm install event-generator falcosecurity/event-generator --namespace "falco"

kubectl get all -n falco
kubectl get pods -n falco
kubectl get svc -n falco

Note: Open ports 2801 and 2802 on the jumpserver and change the username and password for accessing falcosidekick-ui
