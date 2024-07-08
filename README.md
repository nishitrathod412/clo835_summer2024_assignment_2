# clo835_summer2024_assignment_2
Create K8s cluster, deploy containerized stateless applications using K8s manifests, expose the applications as NodePort services, roll out an updated version of the application

## Commands for Setting Up and Deploying Kubernetes Applications

## Generate SSH Key
- ssh-keygen -t rsa -f A2_key

## Terraform Commands
- terraform init
- terraform fmt
- terraform plan
- terraform apply

## Copy Files to EC2 Instance
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key init_kind.sh kind.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key app-deployment.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key app-pod.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key app-replicaset.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key app-service.yaml 54.162.181.19:/tmp

- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key mysql-deployment.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key mysql-pod.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key mysql-replicaset.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key mysql-service.yaml 54.162.181.19:/tmp

- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key app2-deployment.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key app2-pod.yaml 54.162.181.19:/tmp
- scp -i ~/environment/clo835_summer2024_assignment_2/terraform/A2_key app2-service.yaml 54.162.181.19:/tmp

## SSH into EC2 Instance
ssh -i A2_key ec2-user@44.222.226.117

## Inside EC2 Instance
- cd /tmp
- chmod 777 init_kind.sh
- ./init_kind.sh
- kubectl get nodes

- sudo systemctl start docker
- docker ps
- kubectl get all -n kube-system
- kubectl cluster-info
- kubectl describe pod/kube-apiserver-kind-control-plane -n kube-system

- kubectl create namespace app-namespace
- kubectl create namespace mysql-namespace
- kubectl get all -n app-namespace
- kubectl get all -n mysql-namespace

- kubectl apply -f mysql-pod.yaml -n mysql-namespace
- kubectl describe pod mysql -n mysql-namespace
- kubectl apply -f mysql-replicaset.yaml -n mysql-namespace 
- kubectl apply -f mysql-deployment.yaml -n mysql-namespace 
- kubectl apply -f mysql-service.yaml -n mysql-namespace 
- kubectl describe pod mysql -n mysql-namespace

kubectl apply -f app-pod.yaml -n app-namespace
kubectl describe pod app -n app-namespace
kubectl port-forward pod/application-pod 8080:8080 -n app-namespace
curl localhost:8080
kubectl apply -f app-replicaset.yaml -n app-namespace
kubectl apply -f app-deployment.yaml -n app-namespace
kubectl apply -f app-service.yaml -n app-namespace
kubectl logs pod/application-pod -n app-namespace

kubectl apply -f app2-pod.yaml -n app-namespace
kubectl apply -f app2-deployment.yaml -n app-namespace
kubectl apply -f app2-service.yaml -n app-namespace

kubectl describe replicaset app-replicaset -n app-namespace
