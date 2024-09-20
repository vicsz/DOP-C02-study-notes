# DOP-C02-study-notes
Study Notes for AWS Certified DevOps Engineer – Professional (DOP-C02) 

## Useful Links

### Official Exam Guide
- [AWS Certified DevOps Engineer Professional Exam Guide](https://d1.awsstatic.com/training-and-certification/docs-devops/AWS-Certified-DevOps-Engineer-Professional_Exam-Guide.pdf) -- high level overview

### Study Guides / Notes
- [Toward the Cloud - Study Guide](https://towardsthecloud.com/aws-devops-engineer-professional-exam-guide) -- more indepth DOP-C02 overview (compared official guide)
- [Tutorials Dojo DOP-C02 Study Materials](https://tutorialsdojo.com/aws-certified-devops-engineer-professional/) -- useful overview with sample questions and scenarios
- [AWS Devops Engineer Professional](https://certification.kananinirav.com/aws-devops-engineer/) -- including really good summary on [CloudFormation](https://certification.kananinirav.com/aws-devops-engineer/2-infrastructure-as-code/cloudformation.html) -- very through study notes in general -- albeit for v1 of Exam (not v2) 

### Sample Questions
- [AWS Certified DevOps Engineer Professional Sample Questions](https://d1.awsstatic.com/training-and-certification/docs-devops-pro/AWS-Certified-DevOps-Engineer-Professional_Sample-Questions.pdf)

### White Papers & FAQs
- [Infrastructure as Code - AWS Whitepaper](https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf) -- mentioned as a must read for DOP-C02
- [Practicing CI/CD on AWS](https://docs.aws.amazon.com/pdfs/whitepapers/latest/practicing-continuous-integration-continuous-delivery/practicing-continuous-integration-continuous-delivery.pdf) -- solid CI/CD fundamentals overview
- [DevOps on AWS: Best Practices](https://aws.amazon.com/devops/what-is-devops)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Deployment Pipeline Reference Architecture](https://aws-samples.github.io/aws-deployment-pipeline-reference-architecture/) -- good tips / best practices on geenral CI/CD pipeline design
- [AWS Automating safe, hands-off deployments](https://aws.amazon.com/builders-library/automating-safe-hands-off-deployments/)
- [AWS CodeBuild build spec reference](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)

## Quick Study Tips for the AWS DevOps Engineer Professional Exam

### Core AWS DevOps Services
Focus on understanding the **core DevOps services**, such as:
- **AWS CodeCommit**: Managed Git repository for source control.
- **AWS CodeBuild**: Fully managed build service for compiling source code, running tests, and producing software packages.
- **AWS CodePipeline**: Orchestration tool for automating the release pipelines.
- **AWS CodeDeploy**: Service for automating code deployments to EC2, Lambda, or on-premises servers.
- **AWS CloudFormation**: Infrastructure as Code (IaC) tool for provisioning resources.
- **AWS OpsWorks**: Configuration management service using Chef or Puppet.

### Key Concepts to Master
- **Continuous Integration and Continuous Deployment (CI/CD)**: Deep understanding of setting up CI/CD pipelines on AWS.
- **Infrastructure as Code (IaC)**: Master CloudFormation templates, AWS CDK, and how they can be used to deploy infrastructure.
- **Monitoring and Logging**: Know how to use **Amazon CloudWatch**, **AWS X-Ray**, **AWS CloudTrail**, and **AWS Config** for monitoring, tracing, and compliance.
- **Automation and Optimization**: Leverage **AWS Systems Manager**, **Auto Scaling**, and **Elastic Beanstalk** for automating tasks and resource scaling.
- **Security and Compliance**: AWS IAM roles, KMS, Secrets Manager, and GuardDuty are essential for ensuring compliance and secure environments.

## Additional AWS DevOps Services

- **Elastic Beanstalk**: Simplified service for deploying and managing applications. Understand how to use it for automating application deployments.
- **AWS Lambda**: Serverless computing service, useful for automating infrastructure tasks, processing logs, and event-driven architecture.
- **AWS Step Functions**: Orchestrate workflows by coordinating multiple AWS services into serverless workflows.
- **Amazon EventBridge**: Serverless event bus that allows you to route events between AWS services or SaaS applications.
- **Amazon SQS**: Fully managed queuing service for decoupling microservices and ensuring reliable message delivery.
- **Amazon SNS**: Pub-sub messaging service for sending notifications

## Services In-Depth 
### **AWS CodeDeploy Study Sheet**
#### **1. Overview**
- **AWS CodeDeploy** automates the deployment of applications to a variety of targets, including:
  - **EC2 Instances**
  - **On-Premises Servers**
  - **Lambda Functions**
  - **Amazon ECS Services**

- **Deployment Models:**
  - **In-Place Deployments:** Deploys the new application version on existing instances.
  - **Blue/Green Deployments:** Spins up a new environment (green) and switches traffic over from the old environment (blue).

---

#### **2. Deployment Types**

| **Deployment Type** | **Description**                                                                                      | **Pros**                                              | **Cons**                                               |
|---------------------|------------------------------------------------------------------------------------------------------|-------------------------------------------------------|--------------------------------------------------------|
| **In-Place**        | Updates the existing instances with the new revision.                                                 | Simple, no new infrastructure needed.                 | Risk of downtime, possible inconsistencies.             |
| **Blue/Green**      | New instances are launched with the new revision and traffic is shifted to these instances when ready. | Zero downtime, easy rollback.                         | Higher cost, requires managing two environments.        |

---

#### **3. AppSpec File**
- **Definition:** The AppSpec file defines the deployment instructions and hooks for CodeDeploy.
- **Supported Formats:** YAML or JSON.
- **Location:** Must be located in the root directory of the application bundle (in S3, GitHub, etc.).
  
**Key Sections:**
  - **version:** Specifies the version of the AppSpec file.
  - **os:** The target operating system (`linux` or `windows`).
  - **files:** Describes the source and destination paths for files to be copied during deployment.
  - **hooks:** Defines lifecycle event hooks to run scripts or commands during the deployment.

---

#### **4. Lifecycle Hooks (EC2/On-Premises)**

| **Hook**                  | **Description**                                                                                   |
|---------------------------|---------------------------------------------------------------------------------------------------|
| **ApplicationStop**        | Stops the currently running application before the deployment.                                    |
| **BeforeInstall**          | Pre-installation steps like taking backups or stopping services.                                  |
| **AfterInstall**           | Post-installation steps like configuring the environment or setting permissions.                  |
| **ApplicationStart**       | Starts the application after the new version is installed.                                        |
| **ValidateService**        | Verifies that the application is running correctly (e.g., smoke tests).                           |
| **BeforeAllowTraffic**     | Runs before traffic is routed to the new version, typically used for final checks or validations. |
| **AfterAllowTraffic**      | Runs after traffic is routed to the new version, used for cleanup or monitoring tasks.            |

---

#### **5. Rollbacks**
- CodeDeploy can automatically roll back to the previous revision if:
  - Deployment fails (e.g., lifecycle event script fails).
  - Instances fail health checks.
  - You configure **CloudWatch Alarms** to trigger a rollback.

---

#### **6. Deployment Configurations**
- **OneAtATime:** Deploys to one instance at a time.
- **HalfAtATime:** Deploys to 50% of instances at a time.
- **AllAtOnce:** Deploys to all instances at once (fast, but risky for production).

---

#### **7. CodeDeploy Agent**
- For **EC2/On-Premises Deployments**, CodeDeploy requires an agent running on the instance.
- The agent communicates with the CodeDeploy service and executes the deployment instructions as per the AppSpec file.
  
---

#### **8. Monitoring and Logging**
- **CloudWatch Logs:** Capture logs for each lifecycle event and deployment.
- **SNS Notifications:** Can notify via email/SMS if a deployment fails or succeeds.
- **CloudWatch Alarms:** Use alarms to trigger rollbacks or notifications based on deployment health metrics.

---

#### **9. Permissions (IAM)**
- Ensure that:
  - **CodeDeploy Service Role** has the correct permissions to interact with other AWS services (e.g., EC2, Lambda).
  - **EC2 Role** allows the instance to communicate with the CodeDeploy service (for pulling the deployment bundle, running scripts, etc.).

---

#### **10. Sample AppSpec File (YAML)**
```yaml
version: 0.0
os: linux
files:
  - source: /app/source/
    destination: /var/www/myapp
  - source: /config/settings.conf
    destination: /etc/myapp/settings.conf
hooks:
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 600
      runas: root
  AfterInstall:
    - location: scripts/change_permissions.sh
      timeout: 600
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
      runas: root
```

##### **Explanation:**
- **version:** Specifies the version of the AppSpec file (`0.0` is the required value).
- **os:** Specifies the operating system (in this case, `linux`).
- **files:** Specifies the source paths in the deployment bundle and the destination paths on the target instance.
  - **source:** Refers to a directory or file inside the build artifact (e.g., uploaded to S3).
  - **destination:** Specifies the path where the files should be copied on the target instance.
- **hooks:** Defines the deployment lifecycle event hooks and associated scripts.
  - Example: The `ApplicationStop` hook runs the `stop_server.sh` script before stopping the current version of the app.

---

#### **11. Common CodeDeploy Issues**
- **AppSpec File Errors:** Ensure proper syntax (YAML/JSON) and that paths and hooks are correct.
- **Agent Issues:** Ensure the CodeDeploy agent is installed and running on EC2/On-Premises instances.
- **Permission Errors:** Verify IAM roles are correctly assigned to CodeDeploy and EC2 instances.
- **Script Timeouts:** Make sure your lifecycle event scripts are correctly configured with reasonable timeouts.

Here's a **shorter, optimized CodeBuild Study Sheet** in **Markdown** with a concise **sample buildspec.yml** that covers all the key concepts, without redundancy.

---

### **AWS CodeBuild Study Sheet**

#### **1. Overview**
- **AWS CodeBuild** is a fully managed service for **compiling source code**, running tests, and producing artifacts.
- It integrates with **CodePipeline**, **S3**, **CodeDeploy**, and other AWS services for automating CI/CD workflows.

---

#### **2. Key Components**
- **Source**: Code is pulled from sources like **CodeCommit**, **GitHub**, **Bitbucket**, or **S3**.
- **Buildspec File**: YAML file defining the build phases, environment variables, artifacts, caching, and reports.
- **Artifacts**: Output from the build process, stored in **S3** or used by other services.
- **Reports**: Test reports (e.g., JUnit, Cucumber) that can be viewed in the CodeBuild console.
- **Caching**: Reuse dependencies across builds (e.g., `npm`, `maven`), reducing build time.
- **Batch Builds**: Run multiple builds in parallel or sequentially, useful for multi-environment or multi-version workflows.

---

#### **3. Buildspec File Key Sections**
- **version**: Specifies the buildspec version (`0.2` recommended).
- **env**: Defines environment variables, secrets, and proxies.
  - **Secrets Manager**: Injects sensitive data (e.g., `DB_PASSWORD`) from AWS Secrets Manager.
  - **Parameter Store**: Retrieves parameters (e.g., `API_KEY`) from AWS Systems Manager Parameter Store.
- **phases**: The build lifecycle is split into these phases:
  - **install**: Install necessary tools or dependencies.
  - **pre_build**: Setup before the build (e.g., Docker login).
  - **build**: Main build step (e.g., compiling code, running tests).
  - **post_build**: Tasks after build (e.g., packaging artifacts).
  - **finally**: Runs even if earlier phases fail (used for cleanup).
- **artifacts**: Specifies files to store after build (e.g., upload to S3).
- **cache**: Defines paths to cache to speed up subsequent builds (e.g., `maven`, `npm`).
- **reports**: Generate and upload test results for analysis.

---

#### **4. Key Features**
- **IAM Permissions**: CodeBuild needs roles to access services (e.g., S3, Secrets Manager, CodeCommit).
- **Exported Variables**: Environment variables can be defined in one phase and reused in subsequent phases using the `export` command.
- **Proxy Support**: If the build is behind a proxy, configure HTTP/HTTPS proxies via environment variables.
- **Batch Builds**: Execute multiple builds in parallel or sequentially within a single project, ideal for complex workflows.

---

##### **Sample Buildspec File (`buildspec.yml`)**

```yaml
version: 0.2

# Environment variables, including secrets and parameter store.
env:
  variables:
    ENVIRONMENT: production                    # Custom environment variable.
  secrets-manager:
    DB_PASSWORD: my-database-password          # Securely fetch from AWS Secrets Manager.
  parameter-store:
    API_KEY: /myapp/api/key                    # Fetch from Parameter Store.
  proxy:
    HTTP_PROXY: http://proxy.example.com:8080  # Set proxy for build environment.

# Build lifecycle phases.
phases:
  install:
    commands:
      - echo "Installing dependencies..."
      - apt-get update && apt-get install -y awscli
  
  pre_build:
    commands:
      - echo "Logging into Docker..."
      - aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <your_ecr_url>

  build:
    commands:
      - echo "Building the project..."
      - export BUILD_ID=$(uuidgen)             # Exported variable for reuse.
      - mvn clean install                      # Example: Build Java Maven project.

  post_build:
    commands:
      - echo "Packaging build..."
      - zip -r myapp.zip target/               # Package build output.
  
  finally:
    commands:
      - echo "Cleaning up resources..."
      - aws s3 rm s3://my-bucket/temp --recursive  # Example cleanup action.

# Artifacts to upload after the build.
artifacts:
  files:
    - myapp.zip                                # Package to upload.
  base-directory: target                       # Where the output files are stored.

# Reports configuration for test results.
reports:
  test-reports:
    files:
      - '**/*'                                 # Report files pattern.
    base-directory: test-reports               # Directory with test results.

# Caching configuration to speed up future builds.
cache:
  paths:
    - /root/.m2/**/*                           # Cache Maven dependencies.
    - /root/.npm/**/*                          # Cache npm modules.
```

---

#### **5. Summary of Key Concepts**
- **Phases:** Organize your build process into logical steps with **install**, **pre_build**, **build**, **post_build**, and **finally**.
- **Environment Variables:** Leverage **Secrets Manager** and **Parameter Store** for sensitive data, and use exported variables across phases.
- **Caching:** Speed up subsequent builds by caching dependencies.
- **Artifacts & Reports:** Define what gets saved (e.g., build outputs) and generate test reports for visibility.


## Tables

### Deployment Methods

| **Method**             | **Description**                                                                 | **Advantages**                                               | **Challenges**                                                | **Use Cases**                                                 | **AWS Services** (related)                                       |
|------------------------|---------------------------------------------------------------------------------|--------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|------------------------------------------------------------------|
| **Deploy in Place**     | Deploys new code directly onto existing instances or infrastructure.             | Simple, no need for extra infrastructure.                     | Risk of downtime, inconsistent environments during deployment.  | Smaller applications with minimal traffic.                       | Elastic Beanstalk, CodeDeploy (in-place)                          |
| **Rolling Deployment**  | Gradually replaces instances with new ones, while keeping some of the old ones running. | Minimizes downtime, supports graceful replacement.             | Complexity in managing partial deployments, slow rollout.        | Medium- to large-scale applications that need some availability during deployment. | CodeDeploy, Elastic Beanstalk, Auto Scaling                        |
| **Immutable Deployment**| New instances are created with the new version of the application, and old instances are terminated after traffic is routed to the new ones. | Reduces risk of configuration drift, ensures consistency.      | Requires more infrastructure, higher cost.                      | Highly critical applications needing consistent environments.     | CodeDeploy (immutable), Auto Scaling, Elastic Beanstalk            |
| **Traffic Splitting**   | Diverts a small percentage of traffic to the new version before full rollout.    | Minimizes risk, allows testing with live traffic.              | Complex to implement and monitor, needs good rollback mechanisms. | Testing new features with minimal impact, A/B testing.            | AWS App Mesh, Lambda Traffic Shifting, API Gateway, CodeDeploy     |
| **Blue/Green Deployment**| Two separate environments (Blue and Green) are maintained. Traffic is switched to the new environment (Green) when ready. | Zero downtime, easy rollback.                                 | High infrastructure cost, complex environment management.       | Mission-critical systems needing zero downtime during deployment. | Elastic Beanstalk, CodeDeploy, Route 53, ELB, Auto Scaling         |

### Disaster Recovery (DR) Strategies

| **Strategy**           | **Description**                                                                 | **Advantages**                                                 | **Challenges**                                                 | **RPO (Recovery Point Objective)** | **RTO (Recovery Time Objective)** | **AWS Services** (related)                                          |
|------------------------|---------------------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------|--------------------------------------------------------------------|
| **Pilot Light**         | Core services run at minimal capacity in a second region, ready to scale in case of a disaster. | Lower cost than full replication, faster recovery than cold standby. | Must scale up during a disaster, still some downtime.           | Moderate                            | Moderate                            | EC2, RDS (Read Replica), Elastic Load Balancing, Auto Scaling         |
| **Warm Standby**        | A scaled-down version of a fully functional environment is always running, ready to scale up when needed. | Faster recovery than Pilot Light, relatively low cost.          | Requires more resources than Pilot Light, but still some downtime. | Moderate                            | Faster than Pilot Light              | EC2, RDS, Auto Scaling, Elastic Load Balancing                        |
| **Cold Standby**        | Backup is stored, but no active resources are running. The environment is spun up only after a disaster occurs. | Lowest cost.                                                   | Long recovery time, high risk of data loss.                      | High                                | High                                | S3, Glacier, EC2, CloudFormation                                      |
| **Hot Standby (Multi-Site)** | Full-scale version of your application is always running in another region, ready to take over instantly. | Near-instant failover, minimal downtime.                        | Highest cost, complex to maintain.                               | Very Low                            | Very Low                            | Route 53 (failover), EC2, RDS (multi-AZ), Elastic Load Balancing, Auto Scaling |
| **Backup and Restore**  | Data is regularly backed up to a remote location, and infrastructure is spun up from scratch in the event of failure. | Simple and cost-effective.                                     | Slow recovery time, risk of data loss since backups may be infrequent. | High                                | High                                | S3, Glacier, CloudFormation, AWS Backup                               |


## Notes 
- AWS CodeCatalyst is not on the the exam -- given it's recent (2023) introduction.
