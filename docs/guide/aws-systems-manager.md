# AWS Systems Manager

Free service to manage EC2 and On-Premise systems at scale
- Provides patching automation
- Get operational insights & detect problems
- Integrated with AWS Config, CloudWatch metrics & dashboards

Install SSM on systems you want to control
- Installed by default on Amazon Linux & Ubuntu AMI
- Requires proper IAM role to allow SSM actions


## :label: AWS Tags

Tags are text key value pairs that can be added to many AWS resources

- Common tags are Name, Environment, Team, etc.
- Used for automation, resource grouping, cost allocation
- Tag as much as you want, free naming convention!

## :recycle: Resource Groups

Create, view, manage logical groups of tagged resources

- Use SSM to search tags & apply actions across resource groups
- Could be any resources such as:
    - Applications
    - Layers of application state
    - Prod vs. Dev environments
- Regional. Need to create new if in different region

## :clipboard: Documents

Parameters & actions you define with JSON or YAML
- There are many pre-existing documents created by Amazon

[AWS Documents Samples](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-document-samples.html)

## :running: Run Command

Execute document (script) across multiple instances
- Specify parameters & select your targets by tag or instance
- Timeout sets time to wait until script considered failed
- Rate control determines how many targets run at a time
- Output options allow you to write output to S3 or CloudWatch

::: tip Run Command
AWS Systems Manager Run Command lets you remotely and securely manage the configuration of your managed instances. A managed instance is any EC2 instance or on-premises machine in your hybrid environment that has been configured for Systems Manager. Run Command enables you to automate common administrative tasks and perform ad hoc configuration changes at scale. You can use Run Command from the AWS console, the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDKs.

[Run Command @ AWS Docs](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)
:::

## :hammer_and_wrench: Inventory & Patching

Inventory lists software on an instance
- Enable and run AWS-GatherSoftwareInventory to see what's running

Inventory + Run Command lets you patch software

Patch Manager + Maintenance Window is for patching OS

Patch Manager gives you compliance

State Manager helps ensure instances are in a consistent state

:::tip Patch Manager

AWS Systems Manager Patch Manager automates the process of patching managed instances with both security related and other types of updates. You can use Patch Manager to apply patches for both operating systems and applications. You can patch fleets of EC2 instances or your on-premises servers and VM's by operating system type. This includes supported versions of Windows Server, Ubuntu Server, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, CentOS, Amazon Linux, and Amazon Linux 2. You can scan instances to see only a report of missing patches, or you can scan and automatically install all missing patches. 

[Patch Manager @ AWS Docs](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-patch.html)
:::

## :closed_lock_with_key: Parameter Store

Securely store your config & secrets

Optional Seamless Encryption using KMS (Key Management Service)

Serverless, scalable, durable, easy SDK, free

Offers version tracking of config & secrets

Config management using path and IAM

CloudWatch events notifications, and CloudFormation integration

Applications can connect to Parameter Store which checks IAM
- Can be plaintext or encrypted (using KMS)

Parameter Store Hierarchy Example:

(Get Parameters by Path)
- /my-department/
    - my-app/
        - dev/
            - db-url
            - db-password
        - prod/
            - db-url
            - db-password
    - other-app/
- /other-department/

CLI: 
aws ssm get-parameters --names /path/ --with-decryption

aws ssm get-parameters-by-path --path /path/ (query all parameters in path)
- --recursive --with-decryption (optional flags)

::: tip Parameter Store
AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, Amazon Machine Image (AMI) IDs, and license codes as parameter values. You can store values as plain text or encrypted data. You can reference Systems Manager parameters in your scripts, commands, SSM documents, and configuration and automation workflows by using the unique name that you specified when you created the parameter. 

[Parameter Store @ AWS Docs](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)
:::

## :man_cook: Opsworks

An alternative to SSM for managed Chef & Puppet

- Helps consistent deployment by managing configuration as code
- Automates user accounts, cron, ntp, packages, services

Similarities with SSM, Beanstalk, CloudFormation