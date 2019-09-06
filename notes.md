# DevOps **pipeline**

1. Src
2. Build
3. Test
4. Production/Deployment

## AWS tools

1. **_CodeCommit_**
2. **_CodeBuild_**
3. **_Lambdas_** to wrap tests
4. **_CodeDeploy_**

- **_CodePipeline_** : wrapper to the pipeline
- **_CodeStar_** : out of the box template

# Module 1 - DevOps Overview

Devs develop. Ops monitor the product to be a good service, stability.
But when devs make changes, it disrupt stability.
Devs love change. Ops hate change.

DevOps insures that changes don't break stability.

**No manual actions** in the pipeline.

- Continuous Integration: Devs commit frequently (no branching for months)
- Continuous Delivery
- Microservices: break up product in services (SOA: Service Oriented Architecture) => a little change won't imply deploying the whole app => less risks (on other services) **_Amazon Elastic Container Service_**, **_AWS Lambda_**
- Infrastructure as Code **_Cloud Formation_**, **_Beanstalk_**, **_OpsWork_**
- Monitoring and Logging **_AWS CloudWatch_**
- Communication and Collaboration

# Module 2 - **_AWS CLI_**

1 region (ex: Dublin) == at least 2 AZ (Availability Zone) == at least 1 datacenter

EC2 is an on managed service (our responsibility to manage failure/patch/etc.).
EC2 User data can be used to run a script
EC2 tags

Amazon Linux AMI has aws installed

AWS CLI is a CLI tool that can replace the console

- aws
- aws-shell => auto completion

## Lab 1 - Hands-on with **_AWS CLI_**

Powerfull tool, allows scripting

# Module 3 - DevSecOps

- **_AWS Cloudwatch_** metrics (CPU etc. (from security, could detect DDoS)), logs, events
- **_AWS CloudTrail_** captures API calls
- **_AWS Config_** audit configs of other AWS resources, rules (**_SecurityHub_** can give out of the box rules)
  - Infos from those tools could be pushed into an **_S3_** bucket and then analyzed
- **_IAM_** roles, assume a role (both way trust)
- **_AWS Organizations_** to manage roles/accounts
- **_AWS Security Account Structures_**
- **_AWS Systems Manager_** has a Parameter Store to store secret keys/passwords (**free** whereas **_Secret Manager_** is not)
- **_GuarDuty_** lists abnormal activities
- **_Macie_**

## Lab 2 - Building and executing automated **_AWS IAM_** policy tests with **_AWS Config_**

Setting AWS Config rules with Lambdas.

# Module 4 - Deployment Stragtegies and Developer Tools

- In-Place
- Rolling-out
- Blue-Green
- Red-Black (Netflix)
- Immutable

# Module 5 - Infrastructure as Code

Method for automating the process of creating, updating or deleting infrastructure. (With **_CloudFormation_**.)

## Lab 3 - Hands-on with **_AWS CloudFormation_** templates

# Module 6 - Developer Tools

- **_CodeCommit_**: Use Git hooks, has pull request but no built-in Jenkins plugin
- **_CodeBuild_**: yaml config
- **_CodeDeply_**: yaml config
- **_Elastic Beanstalk_**: has its own CLI, allows creating environments

## Lab 4 - Creating a Continuous Delivery Toolchain with **_AWS CodeStar_**

**_CodeStar_** has templates for different apps (nodejs, python etc.) and creates the **_CodePipeline_** with a **_CloudFormation_** template under the hood.

# Module 7 - Tests

Third party tools only. **_CodeBuild_** can/should be used to run them.

# Module 8 - Configuration Management with **_AWS OpsWorks_**, Chef and Puppet

Changes to applications, resources etc keeping a good state. **_OpsWorks_** in **_CodePipeline_** is only available in the us-east-1 region

## Lab 5 - Hands-on with **_AWS OpsWorks_** and Chef

Complicated... But we used **_Lambdas_** !

# Module 9 - AMI Building and **_AWS Systems Manager_**

Tradeoff : os-only AMI with configuration at each instanciation => long boot time VS. fully configured AMI

# Module 10 - Containers: Docker and **_Amazon Elastic Container Service_**

A Docker Container is an isolated environment in which you can run your application. Virtualized environment (not a VM). They share the kernel of the host OS. Technology is _Containerization_.

**_ECS_** orchestrates EC2 instances for running the containers. Either you manage (update, patch, etc.) the EC2 instances, either **_AWS Fargate_** does it for you. **_ECR_** is the Registry (Dockerhub-like).

**_EKS_** for _Kubernetes_ (which is an orchestrer as well).

## Lab 6 - Hands-on with Docker and **_Amazon ECS_**

# Tricks

From an EC2 instance, we can get a lot of info with `curl http://169.254.169.254/latest/meta-data/`

# Services recap

- **_CodePipeline_**
  - source provider : **_CodeCommit_**, **_Amazon ECR_**, **_S3_** or Github (**no Gitlab**)
  - solution for Gitlab:
    1. web hooks
    2. **_API gateway_**
    3. lambda that kicks off **_CodePipeline_**
  - Couldwatch events or **_CodePipeline_** periodic polling
  - Build: **_CodeBuild_** or Jenkins
  - Deploy: lots of solutions
  - A stage can be added at any point with actions in it
    - Action can be anything, even lambdas or manual approve
    - Action groups are sequentials
    - Actions execute in parallel
- **_Step Functions_** coordinates lambdas
- **_CloudFormation_** templates to automate instanciation of stacks
- **_CDK_** will soon arrive and might replace **_CloudFormation_** entirely
- **_Elastic Beanstalk_** create environments
- **_CodeStar_** to create a project (with its own Repo, Pipeline, etc.)
- **_AWS Cloudwatch_** metrics (CPU etc. (from security, could detect DDoS)), logs, events
- **_AWS CloudTrail_** captures API calls
- **_AWS Config_** audit configs of other AWS resources, rules (**_SecurityHub_** can give out of the box rules)
- **_IAM_** roles, assume a role (both way trust)
- **_AWS Organizations_** to manage roles/accounts
- **_AWS Security Account Structures_**
- **_AWS Systems Manager_** management service
- **_GuardDuty_** lists abnormal activities
- **_Macie_**
