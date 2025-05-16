
# Jenkins Interview Questions

---

### **1. What is Jenkins and how have you used it in your CI/CD workflows?**

**ANSWER:**  
Jenkins is an open-source automation server used to implement CI/CD pipelines. I’ve used Jenkins in multiple projects hosted on EC2 instances to automate build, test, and deployment stages for microservices running on EKS and ECS. I’ve integrated it with GitHub for source control, Amazon ECR for image storage, and Kubernetes for deployments.

---

### **2. How do you configure a Jenkins pipeline to build and deploy a Docker container to AWS?**

**ANSWER:**  
I write a `Jenkinsfile` with stages like Checkout, Build, Test, Docker Build, Push to ECR, and Deploy. I authenticate to AWS using credentials or IAM roles, then use the Docker CLI to build and push the image to ECR. For deployment, I use `kubectl apply` or Helm if deploying to EKS. I also configure post-build actions to notify the team via email or Slack.

---

### **3. Have you worked with Jenkins freestyle and pipeline jobs? What are the differences?**

**ANSWER:**  
Yes, I’ve used both. Freestyle jobs are simpler and UI-driven, good for small tasks. Pipeline jobs, defined via `Jenkinsfile`, offer more control and support complex logic, parallel stages, and reusable libraries. I prefer pipeline jobs for production workloads as they are version-controlled and easier to audit.

---

### **4. How do you secure Jenkins?**

**ANSWER:**  
I enable role-based access control (RBAC), restrict anonymous access, and configure user permissions properly. I also use Jenkins credentials store to securely store secrets like AWS keys and DockerHub tokens. I’ve configured webhooks and job triggers using API tokens instead of user credentials.

---

### **5. How do you handle credentials and secrets in Jenkins pipelines?**

**ANSWER:**  
I store credentials in Jenkins using the “Credentials” plugin and access them using `withCredentials` block in a `Jenkinsfile`. For example, I use AWS credentials to authenticate with ECR or kubeconfig to connect to EKS. In some cases, I retrieve secrets from AWS Secrets Manager or Parameter Store within the pipeline.

---

### **6. How have you integrated Jenkins with AWS services like ECR, ECS, or EKS?**

**ANSWER:**  
I use the Jenkins Docker plugin to build images, then use AWS CLI to push them to ECR. For ECS, I update the task definition using `aws ecs update-service`. For EKS, I use `kubectl` or Helm in the pipeline to apply the updated deployment with the new image tag. All of this is scripted inside the `Jenkinsfile`.

---

### **7. How do you implement rollback in Jenkins if a deployment fails?**

**ANSWER:**  
I keep the previous image tag/version in Jenkins or ECR. If a health check fails post-deployment, I trigger a rollback stage to redeploy the previous version. I also use Git versioning in combination with Helm’s rollback feature when using EKS. Jenkins can be configured to detect deployment failures and roll back automatically.

---

### **8. How do you scale Jenkins for large teams or multiple environments?**

**ANSWER:**  
I’ve configured Jenkins master on EC2 with multiple agents (slaves) on separate EC2s using SSH. This helps distribute the workload. I also organize jobs using folders and shared libraries. For multiple environments (dev/test/prod), I parameterize the pipeline and use environment-specific config files or branches.

---

### **9. What are Jenkins shared libraries, and have you used them?**

**ANSWER:**  
Yes. Jenkins shared libraries are reusable Groovy scripts that can be imported across multiple pipelines. I’ve used them to create common deployment logic like Docker build-push steps and notification functions. It reduces duplication and makes pipelines more maintainable.

---

### **10. Have you faced any challenges with Jenkins? How did you resolve them?**

**ANSWER:**  
One challenge was Jenkins running out of disk space due to too many archived builds and Docker layers. I resolved it by adding cleanup steps in the pipeline, limiting build retention, and using a shared EBS volume. Another issue was credential access failing after an IAM policy change, which I debugged using the Jenkins logs and AWS IAM simulation tool.
