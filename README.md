# DSN580S  Assignment: A Single-Node Kubernetes Cluster with Minikube on AWS

**Student Number:** 240497821
**GitHub Repo:** [Your Repo Link Here]

## Task 1: Provision and secure the instance
*   Launched an Ubuntu 26.04 LTS `t3.medium` instance on AWS EC2.
*   Configured Security Group: SSH (Port 22) allowed from My IP only.
*   Connected via SSH: 
    `ssh -i 240497821-assignmnet.pem ubuntu@3.87.244.155`
*   Proof of originality commands:
    `echo "Student Number: 240497821"`
    `date`
    `hostname`

## Task 2: Install Docker, kubectl, and Minikube
*   Installed Docker: `sudo apt install docker-ce docker-ce-cli containerd.io ...`
*   Added user to docker group: `sudo usermod -aG docker $USER`
*   Installed kubectl: `sudo apt-get install -y kubectl`
*   Installed Minikube: `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`
*   Started cluster: `minikube start --driver=docker`
*   Verified: `kubectl get nodes` (showed STATUS: Ready)

## Task 3: Deploy and Verify Workload
*   Applied YAML: `kubectl apply -f nginx-deployment.yaml`
*   Verified pods: `kubectl get pods`
*   Exposed service: `kubectl get svc nginx-service`
*   Scaled up: `kubectl scale deployment nginx-deployment-240497821 --replicas=5`
*   Scaled down: `kubectl scale deployment nginx-deployment-240497821 --replicas=1`
*   Tested self-healing: `kubectl delete pod <pod-name>` (Kubernetes created a new pod immediately).

## Task 4: Connect a Dashboard (Portainer)
*   Installed Portainer via Helm: `helm install portainer portainer/portainer -n portainer --set service.type=NodePort`
*   Used `socat` bridge to access Portainer via AWS Public IP (Port 30777).
*   Captured screenshots of Nodes, Namespaces, Deployments, Pods, and Services.
