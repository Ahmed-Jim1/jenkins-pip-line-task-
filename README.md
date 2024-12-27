# Final Project: Infrastructure Automation and Deployment with CI/CD

## Table of Contents
1. [Task 1: Infrastructure Provisioning with Terraform](#task-1-infrastructure-provisioning-with-terraform)
2. [Task 2: Configuration Management with Ansible](#task-2-configuration-management-with-ansible)
3. [Task 3: Jenkins Pipeline for CI/CD](#task-3-jenkins-pipeline-for-cicd)
4. [Task 4: Deploying to AKS with Argo CD](#task-4-deploying-to-aks-with-argo-cd)
5. [Conclusion](#conclusion)

---

## Task 1: Infrastructure Provisioning with Terraform

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

## Task 2: Configuration Management with Ansible

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

## Task 3: Jenkins Pipeline for CI/CD

### Objective
Automate application build, testing, and deployment to AKS using Jenkins.

### Steps
#### Step 1: Set Up SonarQube with Jenkins
1. Install the **SonarQube Scanner** plugin in Jenkins.
2. Configure SonarQube Server:
   - Navigate to **Manage Jenkins > Configure System > SonarQube Servers**.
   - Add your SonarQube server details and token.
3. Configure the SonarQube Scanner in **Global Tool Configuration**.
4. Validate with a pipeline stage for SonarQube analysis:
   ```groovy
   stage('SonarQube Analysis') {
       steps {
           script {
               withSonarQubeEnv('SonarQube') {
                   sh 'mvn sonar:sonar'
               }
           }
       }
   }
   ```
5. **Validation**: Screenshot of SonarQube analysis results.

#### Step 2: Configure Jenkins Shared Library
1. Create a shared library structure:
   ```plaintext
   shared-library/
       vars/
           checkoutPipeline.groovy
           dockerTasks.groovy
           sonarQubeAnalysis.groovy
           deployToKubernetes.groovy
   ```
2. Add reusable Groovy scripts for each pipeline stage.
3. Configure the shared library in Jenkins:
   - Go to **Manage Jenkins > Configure System > Global Pipeline Libraries**.
   - Add your library name and Git repository URL.
4. Reference the shared library in the Jenkinsfile:
   ```groovy
   @Library('shared-library@main') _
   ```
5. **Validation**: Screenshot of shared library configuration.

#### Step 3: Execute the Jenkins Pipeline
1. Define a complete pipeline in the `Jenkinsfile`:
   ```groovy
   pipeline {
       agent any
       stages {
           stage('Checkout') { steps { checkout scm } }
           stage('Build') { steps { sh 'docker build -t app-image .' } }
           stage('SonarQube Analysis') { steps { script { withSonarQubeEnv('SonarQube') { sh 'mvn sonar:sonar' } } } }
           stage('Push') { steps { sh 'docker push your-docker-repo/app-image' } }
           stage('Deploy') { steps { sh 'kubectl apply -f deployment.yaml' } }
       }
   }
   ```
2. Trigger the pipeline and monitor each stage.
3. Verify the deployment in AKS:
   ```bash
   kubectl get pods
   kubectl get services
   ```
4. **Validation**: Screenshots of pipeline execution and deployed application.

---

## Task 4: Deploying to AKS with Argo CD

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
This project integrates multiple tools and technologies, including Terraform, Ansible, Jenkins, and Argo CD, to create a comprehensive DevOps pipeline. Each task ensures automation, scalability, and reliability for modern application development and deployment.
