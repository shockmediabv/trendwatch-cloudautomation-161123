# trendwatch-cloudautomation-161123
Demo repository for Trendwatch CloudAutomation 2023

Necessary software needed for this demo:
- Debian/Ubuntu based system
- Ansible
- kubectl

Install k3s with the following playbook.
```
ansible-playbook ansible/playbook-k3s.yml
```

Setup your k3s kubeconfig.
```
# Create kube config dir and copy the k3s file.
mkdir ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/k3s.yaml

# Configure kubeconfig and test connection
export KUBECONFIG=~/.kube/k3s.yaml
kubectl get pods -A
```

Install ArgoCD with the Ansible playbook.
```
ansible-playbook ansible/playbook-argocd.yml

# Check if pods are running
kubectl get pods -n argocd
```

Login to ArgoCD
```
# Get admin password
kubectl get secret argocd-initial-admin-secret -n argocd -o go-template='{{ .data.password }}'|base64 -d

# Find service IP. In an production environment this would be connected to an ingress with TLS. But for now we directly connect to the service.
kubectl get service argocd-server -n argocd
```

Apply Application CRD with nginx application. After the apply the application should be visible in the ArgoCD webinterface.
```
kubectl apply -f argocd/application-nginx.yaml
```

Check the new pod(s) in the demo namespace.
```
kubectl get pods -n demo
```
