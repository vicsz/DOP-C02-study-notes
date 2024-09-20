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

#### **5. Sample Buildspec File (`buildspec.yml`)**

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

#### **6. Summary of Key Concepts**
- **Phases:** Organize your build process into logical steps with **install**, **pre_build**, **build**, **post_build**, and **finally**.
- **Environment Variables:** Leverage **Secrets Manager** and **Parameter Store** for sensitive data, and use exported variables across phases.
- **Caching:** Speed up subsequent builds by caching dependencies.
- **Artifacts & Reports:** Define what gets saved (e.g., build outputs) and generate test reports for visibility.

Here’s a **CodePipeline Cheat Sheet** with key concepts relevant for the **AWS Certified DevOps Engineer – Professional (DOP-C02)** exam, along with an example **CodePipeline YAML template** that includes all the key concepts.

---

### **AWS CodePipeline Cheat Sheet**

#### **1. What is AWS CodePipeline?**
- **AWS CodePipeline** is a fully managed continuous integration and continuous delivery (CI/CD) service.
- Automates the build, test, and deploy phases of your release process.
- Integrates with multiple services: **CodeBuild**, **CodeDeploy**, **S3**, **Lambda**, **ECS**, **CloudFormation**, etc.

---

#### **2. Key CodePipeline Concepts**

##### **2.1 Pipeline Stages**
- A **pipeline** consists of **stages**, and each stage has **actions**.
  - **Source Stage**: Fetches code or artifacts (e.g., from **S3**, **CodeCommit**, **GitHub**).
  - **Build Stage**: Compiles, runs tests, and produces artifacts (typically using **CodeBuild**).
  - **Deploy Stage**: Deploys applications (e.g., using **CodeDeploy**, **CloudFormation**, **ECS**).
  - **Approval Stage**: Manual approvals before proceeding to the next stage.

##### **2.2 Actions**
- **Actions** are tasks within a stage. Common action types:
  - **Source**: Pulls code from a repository (e.g., **S3**, **GitHub**, **CodeCommit**).
  - **Build**: Executes a build job (e.g., **CodeBuild**, **Jenkins**).
  - **Test**: Runs test suites.
  - **Deploy**: Deploys artifacts (e.g., **CodeDeploy**, **CloudFormation**, **ECS**).
  - **Invoke**: Calls Lambda functions.
  - **Approval**: Requires manual intervention to approve or reject.

##### **2.3 Pipeline Artifacts**
- **Artifacts** are the output from one action that can be passed to another action (e.g., build output from **CodeBuild** used by a **CodeDeploy** action).

##### **2.4 Pipeline Triggers**
- Pipelines can be triggered by events:
  - **Source Change**: Automatically start when new code is pushed (e.g., to **CodeCommit**, **GitHub**).
  - **Manual Start**: Users can manually start a pipeline.
  - **Schedule**: Start the pipeline at specific intervals.

##### **2.5 Cross-Account Pipelines**
- **Cross-account roles** allow CodePipeline to interact with resources in another AWS account.
- Requires setting up appropriate **IAM roles** with trust policies.

#### **2.6 Encryption**
- Artifacts and the pipeline can be encrypted using **AWS KMS** (Key Management Service).
  
##### **2.7 Retry Behavior**
- CodePipeline retries failed actions automatically. The default behavior is:
  - **1 retry** for most action types.

##### **2.8 Manual Approvals**
- Pipelines can include **manual approval** steps to require human intervention before continuing.
- **SNS notifications** can be sent when an approval is pending.

##### **2.9 Pre/Post Deployment Hooks**
- **CodeDeploy** and **CloudFormation** actions can include pre- and post-deployment lifecycle hooks.
  - Example: A **CodeDeploy** action can trigger `BeforeInstall`, `AfterInstall`, and `ValidateService` hooks.

---

#### **3. CodePipeline Best Practices**
- **Use Multiple Stages**: Break the pipeline into multiple stages to isolate steps (source, build, deploy).
- **Manual Approvals**: Implement approvals for critical environments (e.g., production).
- **Monitoring and Alerts**: Use **CloudWatch Events** or **SNS** to monitor pipeline executions.
- **Cross-Account Pipelines**: Use cross-account pipelines to manage deployments across multiple AWS accounts.

---

#### **4. Example CodePipeline YAML Template**

This template sets up a **basic CI/CD pipeline** with **CodeCommit** as the source, **CodeBuild** for the build process, and **CodeDeploy** to deploy the application.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A basic AWS CodePipeline setup with CodeCommit, CodeBuild, and CodeDeploy.

Resources:

  # IAM Role for CodePipeline with permissions to interact with CodeCommit, CodeBuild, CodeDeploy
  PipelineServiceRole:
    Type: "AWS::IAM::Role"
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: "Allow"
            Principal:
              Service: "codepipeline.amazonaws.com"
            Action: "sts:AssumeRole"
      Policies:
        - PolicyName: "CodePipelinePermissions"
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - Effect: "Allow"
                Action:
                  - "codecommit:*"
                  - "codebuild:*"
                  - "codedeploy:*"
                  - "s3:*"
                  - "iam:PassRole"
                Resource: "*"

  # S3 Bucket to store build artifacts between stages
  CodePipelineArtifactStore:
    Type: "AWS::S3::Bucket"
    Properties:
      VersioningConfiguration:
        Status: "Enabled"

  # CodePipeline Definition
  MyPipeline:
    Type: "AWS::CodePipeline::Pipeline"
    Properties:
      RoleArn: !GetAtt PipelineServiceRole.Arn
      ArtifactStore:
        Type: S3
        Location: !Ref CodePipelineArtifactStore
      Stages:
        # Source Stage: Pulls source code from CodeCommit
        - Name: "Source"
          Actions:
            - Name: "SourceAction"
              ActionTypeId:
                Category: "Source"
                Owner: "AWS"
                Provider: "CodeCommit"
                Version: "1"
              OutputArtifacts:
                - Name: "SourceOutput"
              Configuration:
                RepositoryName: "MyCodeRepo"
                BranchName: "main"
              RunOrder: 1

        # Build Stage: Runs CodeBuild to compile the application
        - Name: "Build"
          Actions:
            - Name: "BuildAction"
              ActionTypeId:
                Category: "Build"
                Owner: "AWS"
                Provider: "CodeBuild"
                Version: "1"
              InputArtifacts:
                - Name: "SourceOutput"
              OutputArtifacts:
                - Name: "BuildOutput"
              Configuration:
                ProjectName: "MyCodeBuildProject"
              RunOrder: 1

        # Deploy Stage: Deploys using CodeDeploy
        - Name: "Deploy"
          Actions:
            - Name: "DeployAction"
              ActionTypeId:
                Category: "Deploy"
                Owner: "AWS"
                Provider: "CodeDeploy"
                Version: "1"
              InputArtifacts:
                - Name: "BuildOutput"
              Configuration:
                ApplicationName: "MyCodeDeployApplication"
                DeploymentGroupName: "MyDeploymentGroup"
              RunOrder: 1

  # CodeBuild Project Definition
  MyCodeBuildProject:
    Type: "AWS::CodeBuild::Project"
    Properties:
      Name: "MyCodeBuildProject"
      ServiceRole: !GetAtt PipelineServiceRole.Arn
      Artifacts:
        Type: "CODEPIPELINE"
      Source:
        Type: "CODEPIPELINE"
      Environment:
        ComputeType: "BUILD_GENERAL1_SMALL"
        Image: "aws/codebuild/standard:5.0"
        Type: "LINUX_CONTAINER"
      TimeoutInMinutes: 30

  # CodeDeploy Application and Deployment Group
  MyCodeDeployApplication:
    Type: "AWS::CodeDeploy::Application"
    Properties:
      ApplicationName: "MyCodeDeployApplication"
  
  MyCodeDeployDeploymentGroup:
    Type: "AWS::CodeDeploy::DeploymentGroup"
    Properties:
      ApplicationName: !Ref MyCodeDeployApplication
      DeploymentGroupName: "MyDeploymentGroup"
      ServiceRoleArn: !GetAtt PipelineServiceRole.Arn
      AutoScalingGroups:
        - "MyAutoScalingGroup"                # Example: Deploy to EC2 ASG
      DeploymentConfigName: "CodeDeployDefault.OneAtATime"
      Ec2TagFilters:
        - Key: "Name"
          Value: "MyEC2Instance"
          Type: "KEY_AND_VALUE"

Outputs:
  PipelineId:
    Description: "The ID of the created pipeline"
    Value: !Ref MyPipeline
```
---

#### **5. Key Concepts Highlighted in the Template**

1. **Stages**: The pipeline has three stages:
   - **Source Stage**: Pulls code from a **CodeCommit** repository.
   - **Build Stage**: Uses **CodeBuild** to build and produce the output artifact.
   - **Deploy Stage**: Uses **CodeDeploy** to deploy the build artifact to EC2 instances (via **Auto Scaling Group**).

2. **IAM Roles**: 
   - **PipelineServiceRole**: Grants permissions for CodePipeline to interact with **CodeCommit**, **CodeBuild**, and **CodeDeploy**.

3. **Artifacts**: 
   - An **S3 bucket** is defined as the artifact store to hold intermediate artifacts between pipeline stages.

4. **Action Configuration**:
   - **SourceAction**: Fetches the code from **CodeCommit**.
   - **BuildAction**: Runs the build in **CodeBuild** using a project called `MyCodeBuildProject`.
   - **DeployAction**: Deploys the output using **CodeDeploy** to a deployment group targeting an Auto Scaling Group.

5. **Cross-Service Integration**:
   - **CodePipeline** integrates with **

CodeCommit**, **CodeBuild**, and **CodeDeploy**, demonstrating multi-service orchestration in a CI/CD pipeline.

6. **Outputs**: Returns the ID of the created pipeline as an output value for reference or further automation.

---

#### **6. Key Concepts for the Exam**
- **Stages and Actions**: Understand how to define and configure multiple stages and their respective actions in a pipeline.
- **Cross-Service Integration**: Know how to integrate **CodeCommit**, **CodeBuild**, **CodeDeploy**, **S3**, and other services.
- **IAM Role Setup**: Ensure proper IAM permissions are granted to CodePipeline to interact with other services.
- **Manual Approvals**: Use **approval actions** to introduce manual approval stages.
- **Artifact Management**: Configure **S3** buckets or **CodeBuild** for managing and storing build artifacts.
- **Notifications**: Use **SNS** for pipeline success, failure, and approval notifications.

---

This **CodePipeline Cheat Sheet** and example template cover the core concepts required for the **AWS Certified DevOps Engineer – Professional (DOP-C02)** exam.

### **AWS CloudFormation Cheat Sheet**

#### **1. What is AWS CloudFormation?**
- **AWS CloudFormation** automates the creation, update, and management of AWS resources through infrastructure-as-code.
- You define resources in **JSON** or **YAML** templates.

#### **2. CloudFormation Key Concepts**

##### **2.1 Parameters**
- **Parameters** allow you to pass values (like environment, instance type) into the template, making it reusable and configurable.
  
##### **2.2 Pseudo Parameters**
- **Pseudo Parameters** are pre-defined by CloudFormation, and provide contextual information.
  - **Examples:** `AWS::Region`, `AWS::AccountId`, `AWS::StackName`.

##### **2.3 Conditions**
- **Conditions** determine whether certain resources or properties are created/assigned based on parameter values or pseudo parameters.

##### **2.4 Intrinsic Functions**
- **Intrinsic Functions** allow dynamic configuration inside the template:
  - **Fn::Sub:** String substitution for variables (e.g., `"MyApp-${Environment}"`).
  - **Fn::Join:** Concatenate values with a delimiter.
  - **Fn::GetAtt:** Fetch attributes (like ARN or DNS) from resources.
  - **Fn::If:** Conditional logic inside the template.
  - **Fn::FindInMap:** Retrieve values from a `Mappings` section.

##### **2.5 Mappings**
- **Mappings** store key-value pairs used for region-specific values or configuration.

##### **2.6 Outputs**
- **Outputs** return information after the stack is created (e.g., instance ID, public DNS).

##### **2.7 Deletion Policies**
- **DeletionPolicy**: Retain or create snapshots of resources before deletion.

---

#### **3. CloudFormation Best Practices**
- **Use Parameters**: Make templates flexible and reusable.
- **Use Conditions**: Dynamically create resources based on input or environment.
- **Use Outputs**: Share information like instance IDs and ARNs across stacks.
- **Use `Mappings`**: For region or environment-specific configurations.
- **Stack Policies & Change Sets**: Control updates and safely preview changes.

---

#### **4. Sample CloudFormation Template**

Here’s a single **CloudFormation template** in **YAML** that includes **parameters**, **conditions**, **intrinsic functions**, **pseudo parameters**, **mappings**, **outputs**, and more.

```yaml
AWSTemplateFormatVersion: '2010-09-09'

Description: >
  CloudFormation template demonstrating key concepts for AWS Certified DevOps Engineer exam.

# Parameters section defines inputs from the user.
Parameters:
  Environment:
    Description: "Specify the environment (prod or dev)"
    Type: String
    Default: dev
    AllowedValues: 
      - prod
      - dev
    ConstraintDescription: "Must be either prod or dev."

  InstanceType:
    Description: "EC2 instance type"
    Type: String
    Default: t2.micro
    AllowedValues: 
      - t2.micro
      - t2.small
      - t2.medium
    ConstraintDescription: "Must be a valid EC2 instance type."

# Mappings section for region-specific AMI IDs.
Mappings:
  RegionMap:
    us-east-1:
      AMI: "ami-0abcdef1234567890"
    us-west-2:
      AMI: "ami-0987654321abcdef0"

# Conditions section to create resources based on input values.
Conditions:
  IsProdEnvironment: !Equals [!Ref Environment, "prod"]

# Resources section defines the AWS infrastructure.
Resources:
  MyEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      InstanceType: !Ref InstanceType           # Referencing the InstanceType parameter
      ImageId: !FindInMap [RegionMap, !Ref AWS::Region, AMI]  # Lookup AMI based on region
      SecurityGroupIds: 
        - !Ref MySecurityGroup                 # Attach the security group
      Tags:
        - Key: "Name"
          Value: !Sub "MyInstance-${Environment}"   # String substitution for dynamic name

  MySecurityGroup:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: "Allow SSH access"
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0                    # Open port 22 (SSH) to the world
  
  ProdEC2Instance:
    Condition: IsProdEnvironment                # Only create this resource if the environment is "prod"
    Type: "AWS::EC2::Instance"
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: !FindInMap [RegionMap, !Ref AWS::Region, AMI]
      SecurityGroupIds: 
        - !Ref MySecurityGroup

# Outputs section returns useful information like the instance ID and public DNS.
Outputs:
  EC2InstanceID:
    Description: "ID of the created EC2 instance"
    Value: !Ref MyEC2Instance
  InstancePublicDNS:
    Description: "Public DNS of the EC2 instance"
    Value: !GetAtt MyEC2Instance.PublicDnsName
```

---

#### **5. Key Concepts Highlighted in the Template**

1. **Parameters:**
   - `Environment` (with `prod` and `dev` options) and `InstanceType` for user input.

2. **Mappings:**
   - `RegionMap` for region-specific AMIs (e.g., different AMIs for `us-east-1` and `us-west-2`).

3. **Conditions:**
   - `IsProdEnvironment` condition ensures that certain resources (like `ProdEC2Instance`) are only created in the production environment.

4. **Intrinsic Functions:**
   - `!Ref` references parameter values (e.g., `InstanceType`).
   - `!FindInMap` retrieves region-specific values from the `RegionMap`.
   - `!Sub` performs string substitution (e.g., dynamic naming for EC2 instances).
   - `!GetAtt` retrieves the **Public DNS** of the EC2 instance.

5. **Outputs:**
   - **Instance ID** and **Public DNS** are returned as outputs.

6. **Conditions with Resources:**
   - `ProdEC2Instance` is created only when the `Environment` is `prod`, showing how to use conditions to manage infrastructure per environment.

---

#### **6. Key Concepts for the Exam**
- **Intrinsic Functions**: Learn how to use `!Ref`, `!GetAtt`, `!Sub`, `!FindInMap`, and conditionals like `!If`.
- **Parameters**: Make templates flexible with user input for instance types, environment, etc.
- **Conditions**: Dynamically create resources based on environment (e.g., `prod`, `dev`).
- **Mappings**: Static region-specific or environment-specific configuration (e.g., AMIs).
- **Outputs**: Return useful information like instance IDs, public DNS, or ARNs.
- **Pseudo Parameters**: Use built-in values like `AWS::Region`, `AWS::AccountId`, and `AWS::StackName`.


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
