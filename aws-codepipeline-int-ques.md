# AWS CodePipeline

### 1. What is AWS CodePipeline and how have you used it in your projects?
**AWS CodePipeline** is a fully managed continuous integration and continuous delivery (CI/CD) service that automates the build, test, and deployment phases of application release processes.

**How I’ve used it:**
- Automating deployments for microservices hosted in ECS, Lambda, and EC2.
- Integrating with GitHub for source, CodeBuild for testing/building, and CodeDeploy for deployment.
- Supporting blue/green and canary deployments.

### 2. Describe the structure of a typical pipeline you've built (stages like source, build, deploy).
**Typical Pipeline Structure:**
1. **Source**: Triggered by changes in GitHub/CodeCommit.
2. **Build**: Uses CodeBuild to run tests, lint code, and package artifacts.
3. **Approval (optional)**: Manual approval for production deployments.
4. **Deploy**: 
   - Lambda: direct deployment using CodePipeline.
   - ECS: task definition update with CodeDeploy.
   - EC2: rolling or blue/green deployments via CodeDeploy.

### 3. How do you integrate CodePipeline with CodeBuild, CodeDeploy, or Lambda?
- **CodeBuild**: Defined in the `buildspec.yml` or inline in pipeline definition. It outputs build artifacts that feed into deploy stages.
- **CodeDeploy**: Set up as a deployment stage. You define application, deployment group, and specify AppSpec files.
- **Lambda**: Direct integration via Lambda deployment action type. Artifacts are zipped and deployed directly to the specified Lambda function.

### 4. How do you trigger CodePipeline automatically on source code changes?
- Using **webhooks** for GitHub or **CloudWatch Events** for CodeCommit.
- CodePipeline auto-creates a webhook when GitHub is set as the source.
- For S3, object uploads trigger the pipeline via event notification.

### 5. Have you used CodePipeline with third-party tools like GitHub or Jenkins?
Yes:
- **GitHub**: As the primary source stage. It supports webhook-based triggering.
- **Jenkins**: Integrated using a custom action or invoking Jenkins jobs from a Lambda or CodeBuild step. This is useful when complex build logic already exists in Jenkins.

### 6. How do you manage environment-specific deployments in CodePipeline?
- Separate pipelines per environment (e.g., `dev`, `staging`, `prod`).
- Use **parameterized templates** in CloudFormation or CodeDeploy.
- Employ **pipeline variables**, environment-specific buildspec files, or conditionals in the code.
- Incorporate **manual approval** for promoting from staging to production.

### 7. How do you handle failure in any stage of the pipeline? Can you configure rollback or manual approval?
- **Failures** stop the pipeline; logs are viewable in CloudWatch and CodeBuild logs.
- **Rollback**:
  - CodeDeploy supports automatic rollback if a deployment fails.
  - For Lambda and ECS, you can deploy previous artifacts manually or automate it.
- **Manual approvals** are integrated before critical stages using the `Approval` action in the pipeline.

### 8. How do you secure access to CodePipeline and limit who can trigger or modify it?
- Use **IAM roles and policies** to control access.
- Grant least privilege to users and service roles.
- Use **resource-level permissions** for pipelines.
- Enable **AWS CloudTrail** for auditing changes.
- Use **approval gates** to prevent unintended deployments to sensitive environments.

### 9. Can you use CodePipeline for multi-region or multi-account deployments? How?
Yes:
- Use **cross-account roles** and **resource sharing**.
- Deploy CloudFormation stacks or artifacts across regions/accounts by assuming IAM roles.
- You may use **Step Functions** or **custom Lambda actions** in the pipeline for orchestrating complex flows.
- Artifacts can be stored in **S3 buckets with cross-region replication**.

### 10. What’s a real issue you faced while working with CodePipeline and how did you resolve it?
**Issue:** Deployment to production was triggered accidentally due to a misconfigured webhook.

**Resolution:**
- Added a **manual approval** before the production stage.
- Implemented **branch-based filters** in the source stage to only trigger pipelines from `main` or `release/*`.
- Added **Slack notifications** via SNS to alert the team when deployments begin and complete.
