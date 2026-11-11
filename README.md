# Java Web Application (Docker, Kubernetes, AWS EKS)

## Steps to Deploy

1. **Build the Application: command**
   ```bash
   mvn clean package
   ```

2. **Build and Push Docker Image:**
   ```bash
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
   aws ecr create-repository --repository-name java-web-app
   docker build -t java-web-app .
   docker tag java-web-app:latest <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/java-web-app:latest
   docker push <YOUR_AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/java-web-app:latest
   ```

3. **Deploy to Kubernetes:**
   Update `<YOUR_AWS_ACCOUNT_ID>` inside `k8s-deployment.yaml` and execute:
   ```bash
   eksctl create cluster --name demo-cluster --region us-east-1 --node-type t3.medium --nodes 2
   kubectl apply -f k8s-deployment.yaml
   kubectl get service java-web-service
   ```
