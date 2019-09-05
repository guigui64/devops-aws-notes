# DevOps **pipeline**

1. Src
2. Build
3. Test
4. Production/Deployment

## AWS tools

1. _CodeCommit_
2. _CodeBuild_
3. _Lambdas_ to wrap tests
4. _CodeDeploy_

- _CodePipeline_ : wrapper to the pipeline
- _CodeStar_ : out of the box template

# Module 1 DevOps Overview

Devs develop. Ops monitor the product to be a good service, stability.
But when devs make changes, it disrupt stability.
Devs love change. Ops hate change.

DevOps insures that changes don't break stability.

**No manual actions** in the pipeline.

- Continuous Integration: Devs commit frequently (no branching for months)
- Continuous Delivery
- Microservices: break up product in services (SOA: Service Oriented Architecture) => a little change won't imply deploying the whole app => less risks (on other services) _Amazon Elastic Container Service_, _AWS Lambda_
- Infrastructure as Code _Cloud Formation_, _Beanstalk_, _OpsWork_
- Monitoring and Logging _AWS CloudWatch_
- Communication and Collaboration

# Module 2 AWS CLI

1 region (ex: Dublin) == at least 2 AZ (Availability Zone) == at least 1 datacenter

EC2 is an on managed service (our responsibility to manage failure/patch/etc.).
EC2 User data can be used to run a script
EC2 tags

Amazon Linux AMI has aws installed

AWS CLI is a CLI tool that can replace the console

- aws
- aws-shell => auto completion

## Lab 1 CLI

Cheatsheet

```
aws ec2 describe-instances
aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId'
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,Tags]'
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value, IamInstanceProfile.Arn]'
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]'
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]' --output table
aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib*" --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0], IamInstanceProfile.Arn]' --output table
appROLEARN=$(aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib Web*" --query 'Reservations[0].Instances[0].IamInstanceProfile.Arn' --output text)
aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$appROLEARN'].Roles[0].RoleName"
appROLENAME=$(aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$appROLEARN'].Roles[0].RoleName" --output text)
aws iam list-role-policies --role-name $appROLENAME
appPOLNAME=$(aws iam list-role-policies --role-name $appROLENAME --query PolicyNames[] --output text)
aws iam get-role-policy --role-name $appROLENAME --policy-name $appPOLNAME
apiROLEARN=$(aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib API*" --query 'Reservations[0].Instances[0].IamInstanceProfile.Arn' --output text)
apiROLENAME=$(aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$apiROLEARN'].Roles[0].RoleName" --output text)
apiPOLNAME=$(aws iam list-role-policies --role-name $apiROLENAME --query PolicyNames[] --output text)
aws iam get-role-policy --role-name $apiROLENAME --policy-name $apiPOLNAME
saveROLEARN=$(aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib Save*" --query 'Reservations[0].Instances[0].IamInstanceProfile.Arn' --output text)
saveROLENAME=$(aws iam list-instance-profiles --query "InstanceProfiles[?Arn=='$saveROLEARN'].Roles[0].RoleName" --output text)
savePOLNAME=$(aws iam list-role-policies --role-name $saveROLENAME --query PolicyNames[] --output text)
aws iam get-role-policy --role-name $saveROLENAME --policy-name $savePOLNAME
aws elasticbeanstalk describe-applications
aws opsworks describe-stacks
aws deploy list-applications
aws deploy list-deployments
DEPLOYARRAY=$(aws deploy list-deployments --output text)
IFS=' ' read -r -a DEPLOYID <<< $DEPLOYARRAY
echo "${DEPLOYID[1]}"
echo "${DEPLOYID[3]}"
echo "${DEPLOYID[5]}"
aws deploy list-deployment-instances --deployment-id ${DEPLOYID[1]}
aws ec2 describe-instances --filter "Name=tag:Name,Values=MadLib*" --query 'Reservations[].Instances[].[InstanceId, Tags[?Key==`Name`].Value | [0]]' --output table
aws deploy get-deployment --deployment-id ${DEPLOYID[1]}
```

# Module 3 DevSecOps

- _AWS Cloudwatch_ metrics (CPU etc. (from security, could detect DDoS)), logs, events
- _AWS CloudTrail_ captures API calls
- _AWS Config_ audit configs of other AWS resources, rules (_SecurityHub_ can give out of the box rules)
  - Infos from those tools could be pushed into an _S3_ bucket and then analyzed
- _IAM_ roles, assume a role (both way trust)
- _AWS Organizations_ to manage roles/accounts
- _AWS Security Account Structures_
- _AWS Systems Manager_ Parameter Store to store secret keys/passwords (**free** whereas _Secret Manager_ is not
- _GuarDuty_ lists abnormal activities)
- _Macie_

## Lab 2 - Building and executing automated AWS IAM policy tests with AWS Config

Setting AWS Config rules with Lambdas.

# Module 4 Deployment Stragtegies and Developer Tools

- In-Place
- Rolling-out
- Blue-Green
- Red-Black (Netflix)
- Immutable

# Module 5 Infrastructure as Code

Method for automating the process of creating, updating or deleting infrastructure.

_CloudFormation_ templates to automate instanciation of stacks.

## Lab 3 - Hands-on with AWS CloudFormation templates

# Module 6 Developer Tools

- _CodeCommit_: Use Git hooks, has pull request but no built-in Jenkins plugin
- _CodeBuild_: yaml config
- _CodeDeply_: yaml config
- _Elastic Beanstalk_: has its own CLI, allows creating environments

## Lab 4 - Creating a Continuous Delivery Toolchain with AWS CodeStar

_CodeStar_ has templates for different apps (nodejs, python etc.)

# Module 7 Tests

Third party tools only. _CodeBuild_ can/should be used to run them.

# Tricks

From an EC2 instance, we can get a lot of info with `curl http://169.254.169.254/latest/meta-data/`

# Services recap

- _Config_ watches whole AWS resources linked to an account. More for **Security**
- _CodePipeline_
  - source provider : _CodeCommit_, _Amazon ECR_, _S3_ or Github (**no Gitlab**)
  - solutions for Gitlab:
    - web hooks
    - _API gateway_
    - lambda that kicks off _CodePipeline_
  - Couldwatch events or _CodePipeline_ periodic polling
  - Build: _CodeBuild_ or Jenkins
  - Deploy: lots of solutions
  - A stage can be added at any point with actions in it
    - Action can be anything, even lambdas or manual approve
    - Action groups are sequentials
    - Actions execute in parallel
- _Step Functions_ coordinates lambdas
