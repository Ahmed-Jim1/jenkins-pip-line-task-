# Final Project: Infrastructure Automation and Deployment with CI/CD

## Table of Contents
1. [Task 1: Containerization with Docker](#task-1-containerization-with-docker)
2. [Task 2: Infrastructure Provisioning with Terraform](#task-2-infrastructure-provisioning-with-terraform)
3. [Task 3: Jenkins Pipeline for CI/CD](#task-3-jenkins-pipeline-for-cicd)
4. [Task 4: Ansible for Configuration Management](#task-4-ansible-for-configuration-management)
5. [Task 5: Integrating Argo CD with AKS](#task-5-integrating-argo-cd-with-aks)
6. [Conclusion](#conclusion)

---

## Task 1: Containerization with Docker

### Objective
Containerize a Java application using Docker for easier deployment and scalability.

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/Ibrahim-Adell/FinalProjectCode.git
   cd FinalProjectCode
   ```
2. Build the Docker image:
   ```bash
   docker build -t my-java-app:latest .
   ```
3. Verify the Docker image:
   ```bash
   docker images
   ```
4. Run the container:
   ```bash
   docker run -d -p 8080:8080 my-java-app:latest
   ```

### Validation
- **Command Outputs**: Screenshot of `docker images` and `docker ps` showing the running container.
- **Application Access**: Screenshot of the application running on `http://localhost:8080`.

---

## Task 2: Infrastructure Provisioning with Terraform

### Objective
Provision AWS resources including VPC, subnets, security groups, and an EC2 instance using Terraform.

### Steps
1. Navigate to the Terraform directory:
   ```bash
   cd terraform
   ```
2. Initialize Terraform:
   ```bash
   terraform init
   ```
3. Plan the infrastructure:
   ```bash
   terraform plan
   ```
4. Apply the configuration:
   ```bash
   terraform apply
   ```
5. Verify resources in the AWS Management Console.

### Validation
- **Command Outputs**: Screenshot of `terraform plan` and `terraform apply`.
- **AWS Resources**: Screenshot of VPC, subnets, and EC2 instances in the AWS Console.

---

## Task 3: Jenkins Pipeline for CI/CD

### Objective
Automate application build, testing, and deployment to AKS using Jenkins.

### Steps
1. Configure the Jenkins pipeline:
   - Add the `Jenkinsfile` to your repository.
   - Use shared libraries for reusable stages.
2. Execute the pipeline in Jenkins.
   ```groovy
   pipeline {
       stages {
           stage('Checkout') { steps { checkout scm } }
           stage('Build') { steps { sh 'docker build -t app-image .' } }
           stage('Push') { steps { sh 'docker push your-docker-repo/app-image' } }
           stage('Deploy') { steps { sh 'kubectl apply -f deployment.yaml' } }
       }
   }
   ```
3. Verify deployment in AKS:
   ```bash
   kubectl get pods
   kubectl get services
   ```

### Validation
- **Command Outputs**: Screenshots of Jenkins pipeline stages passing.
- **AKS Deployment**: Screenshots of Kubernetes pods and services.

---

## Task 4: Ansible for Configuration Management

### Objective
Configure EC2 instances for Jenkins, Docker, SonarQube, and Kubernetes CLI using Ansible.

### Steps
1. Set up Ansible inventory:
   ```ini
   [master]
   master-node ansible_host=<IP> ansible_user=ec2-user

   [slave]
   slave-node ansible_host=<IP> ansible_user=ec2-user
   ```
2. Run playbooks:
   - Master node:
     ```bash
     ansible-playbook -i inventory master/playbook.yml
     ```
   - Slave node:
     ```bash
     ansible-playbook -i inventory slave/playbook.yml
     ```
3. Verify configurations on the EC2 instances.

### Validation
- **Command Outputs**: Screenshot of successful Ansible playbook execution.
- **Node Configurations**: Screenshots of Jenkins, Docker, and SonarQube running on respective nodes.

---

## Task 5: Integrating Argo CD with AKS

### Objective
Deploy applications to AKS using Argo CD for continuous delivery.

### Steps
1. Install Argo CD CLI:
   ```bash
   brew install argocd
   ```
2. Create a namespace for Argo CD:
   ```bash
   kubectl create namespace argocd
   ```
3. Install Argo CD:
   ```bash
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
4. Access the Argo CD server:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```
   Open `https://localhost:8080` in your browser.
5. Login to Argo CD:
   ```bash
   argocd login localhost:8080
   ```
6. Deploy an application:
   ```bash
   argocd app create my-app \
       --repo https://github.com/Ibrahim-Adell/FinalProjectCode.git \
       --path manifests \
       --dest-server https://kubernetes.default.svc \
       --dest-namespace default
   ```
7. Sync the application:
   ```bash
   argocd app sync my-app
   ```
8. Verify the deployment:
   ```bash
   kubectl get pods
   kubectl get services
   ```

### Validation
- **Command Outputs**: Screenshot of `argocd app sync` and application status.
- **Argo CD UI**: Screenshot of the application dashboard in Argo CD.

---

## Conclusion
This project integrates multiple tools and technologies, including Docker, Terraform, Jenkins, Ansible, and Argo CD, to create a comprehensive DevOps pipeline. Each task ensures automation, scalability, and reliability for modern application development and deployment.
