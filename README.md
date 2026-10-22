# DSN580S Assignment: A Single-Node Kubernetes Cluster with Minikube on AWS

Student Number: 240497821

Student Name: Kamva Ntlanga

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

## Task 5: Reflection

**Single Point of Failure:** In this single-node cluster, the AWS EC2 instance acts as the control plane and the worker node simultaneously. If the VM crashes, loses network connectivity, or runs out of memory, the entire cluster fails. All Nginx pods will go offline, and the service will become unreachable. This illustrates the distributed systems problem of a Single Point of Failure (SPOF). There is no fault tolerance because there is no redundancy.

**Service Discovery:** The Kubernetes Service (`nginx-service`) provides a stable IP address and DNS name, abstracting the dynamic nature of the Pods. When I deleted a pod during the self-healing test, the ReplicaSet created a new pod with a completely new IP address. However, the Service automatically updated its endpoints to route traffic to the new pod, meaning the user never noticed a change. This connects to the course concepts of service discovery and decoupling service consumers from the ephemeral lifecycle of microservice instances.

**Multi-node Clusters:** A real, multi-node cluster would add high availability. The control plane components (like etcd and kube-apiserver) could run on multiple nodes using leader election, preventing a single failure from killing the control plane. The scheduler would distribute pods across multiple worker nodes, meaning a hardware failure on one node would only affect the pods on that node, while replicas on other nodes keep the application running. However, this introduces new distributed system problems, such as network partitions (split-brain), the complexity of keeping etcd consistent across nodes, and the need for load balancing across different physical machines.

References:

Kubernetes Documentation. Services, Load Balancing, and Networking. Available at: https://kubernetes.io/docs/concepts/services-networking/service/

Kubernetes Documentation. Nodes. Available at: https://kubernetes.io/docs/concepts/architecture/nodes/
