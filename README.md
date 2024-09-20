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
