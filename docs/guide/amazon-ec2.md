# Amazon EC2

## :rocket: Launching an EC2 Instance

Launching a new EC2 instance is fairly straight forward.

1. Choose Amazon Machine Image (AMI)
    - For your first instance try Amazon Linux 2 AMI
2. Choose an Instance Type
    - t2.micro instance is free tier eligible
3. Configure Instance Details
4. Add Storage
    - Allows you to configure EBS volume used at launch
5. Add Name Tag to Identify Your EC2 Instance
6. Configure Security Group
    - Set of firewall rules for your instance
7. Review and Launch Instance
    - Requires you to select a key pair for remote access

::: tip SSH to Amazon Linux 2 Instance
```
[~/aws] chmod 0400 AWSkey.pem
[~/aws] ssh ec2-user@server.ipaddress -i AWSkey.pem
```
:::

## :computer: Changing EC2 Instance Type

1. Right click on instance, select Instance State > Stop
2. Right click on instance, select Instance Settings > Change Instance Type

::: tip Tips
- The same EBS volumes will remain attached
- The instance will be assigned a new public IPv4
- Behind the scenes AWS may be moving the instance to other hardware 
:::

## :1st_place_medal: EC2 Placement Groups

Placement groups control how instances are placed within AWS infrastructure.


### **Cluster**
- Instances grouped together in low-latency single Availability Zone
- Pro: Strong 10Gbps network with super low latency
- Con: If rack fails all instances fail
- Example usage: Complete big data job quickly. Low latency application

![Cluster Placement Group](/cluster-placement-group.png)

### **Spread** 
- Instances spread across different hardware with at most 7 per group per AZ
- Pros: Can span Availability Zones, Instances on different physical hardware
- Con: 7 instance per AZ per placement group limit
- Example usage: Critical apps (isolated instance failure) Maximizing high availability
- Example image shows 7 instances placed on seven different racks

![Cluster Placement Group](/spread-placement-group.png)

### **Partition**
- Instances spread across different partitions on different racks within AZ
- Can scale to hundreds of EC2 Instances (Hadoop, Kafka, Cassandra)
- Example shows each partition with multiple instances on different racks

![Cluster Placement Group](/partition-placement-group.png)


## :no_entry_sign: EC2 Shutdown & Termination

### Shutdown Behavior
- What happens when shutdown from within the instance OS
- By default "Stopped", can also be "Terminated"

### Termination Protection
- Protects against accidental termination in AWS console or CLI

::: warning
When instance shutdown behavior = terminate and Enable Terminate Protection ON:

If you shutdown the instance from the OS the instance will still be terminated!
:::

## :question: Troubleshooting EC2 Launch

### InstanceLimitExceeded

Error that you have reached your maximum number of instances in a region

Default limit of instances per region is 20, increase can be requested

### InsufficientInstanceCapacity

Error results from AWS not having enough on-demand capacity in specified AZ

Resolution
- Request launch again after waiting a few minutes
- If requesting multiple instances, request one at a time
- Request a different instance type for now and resize it later

### Immediate Termination

If instance goes from pending to terminated immediately it could be:
- EBS volume limit reached
- EBS snapshot is corrupt
- Root EBS volume is encrypted and you don't have KMS key access permission
- Instance store backed AMI is missing a required part (image.part.xx)

Investigate State transition reason in AWS console instance Description tab

::: tip State Transition Columns
Click on the :gear: at top of AWS instance console

You can add State Transition Reason & Message columns
:::

## :grey_question: Troubleshooting EC2 SSH

### Unprotected Private Key

Ensure private key .pem file on your terminal has 400 permissions

### Host key not found

Ensure the username for the OS is correct
- Amazon Linux 2 AMI: ec2-user

### Connection Timeout

Incorrect security group configuration or CPU load is too high

## :desktop_computer: EC2 Instance Types

Comparison tool: [EC2intances.info](https://www.ec2instances.info/)

### T - Burst

T2/T3 Burstable instances have normal CPU performance

- When load spikes it can burst the CPU for increased performance
- Credits used when machine bursts, performance becomes poor if all used
- Burst credits accrue over time when machine is not bursting
- If credits consistently low, move to non burstable

T2/T3 Unlimited

- Charges more if you go over credit balance, but no loss in performance

### R - RAM

Apps that need high RAM like in-memory caches

### C - CPU

Apps that need good CPU computer and databases

### M - Medium

Apps that are balanced, general web apps

### I - I/O

Apps thats need good local I/O instances storage like databases

### G - GPU

Apps thats need a GPU like video rendering and machine learning


## :small_airplane: Instance Launch Types

### On Demand Instances

Short term workloads with predictable pricing

- Pay for what you use (per second, after first minute)
- Highest cost but no upfront payment or long term commitment
- Good for short term un-interrupted workloads

### Reserved Instances

Long workloads of a year or more

- As much as 75% discount to On Demand
- Pay upfront for long term commitment
- Reservation period can be 1 or 3 years

#### Convertible Reserved

- Allows you to change the instance type
- As much as 54% discount to On Demand

#### Scheduled Reserved

- Launch within specific reserved time window

### Spot Instances

Short cheap workloads which can be interrupted

- As much as 90% discount to On Demand
- Bidding system with varied price based on offer & demand
- Reclaimed with 2 minute warning when spot price above your bid
- Good for batch jobs, big data analysis, workloads OK to fail
- Not good for critical jobs and databases

### Dedicated Instances

No other customers share your hardware

- Per instance billing & automatic instance placement

### Dedicated Hosts

A physical server with full control of instance placement

- Per host billing
- Visible underlying sockets & physical cores
- Reservation period of 3 years
- Used for software with complex licensing model or strong regulatory needs


## :cd: Amazon Machine Images

AWS comes with base images such as
- Ubuntu, Fedora, Red Hat, Windows, etc

Images can be customized at runtime.

You can create your own custom AMI's or use other's optimized AMI
- Pre installed packages or apps
- Faster boot time
- Pre configured monitoring or software
- Security controls
- Controlling maintenance and updates
- Active Directory Integration
- AMI's are built for a specific AWS region

You can pay for public AMI's by the hour
- Optimized software
- Easy to configure & run
- Like renting expertise from the AMI author

AMI's take space and reside in Amazon S3
- Private by default, locked for account & region
- Charged for space taken in S3


## :incoming_envelope: Copy AMI Across Accounts

AMI's can be shared with other AWS accounts
- Doesn't affect ownership of the AMI
- If you copy a shared AMI, you become the owner.

To copy; owner must grant read permissions for storage backing the AMI

You can't copy encrypted AMI shared from another account
- You can copy the snapshot and re-encrypt with your own key
- You can register the snapshot copy as a new AMI

You can't copy AMI with billingProduct code share from another account
- Including Windows & AWS Marketplace AMI's
- To copy launch EC2 using shared AMI and create AMI from instance

## :link: Elastic IP Address

When you stop and start EC2 instances the public IP changes

Use an Elastic IP to have a fixed public IP

- Can be attached to one instance at a time
- Can be remapped across instances

You only pay for the Elastic IP when it's not attached to a server

Why use an Elastic IP?
- You need a static IP
- Masking failure of instance or app through rapid remap to other instance

You can have up to 5 Elastic IP's (can request increase)

Avoid using Elastic IP's
- Use Route 53 to register a DNS name to an instance
- Use a load balancer with a static hostname

## :chart_with_upwards_trend: EC2 Metrics & Logs

AWS Provided CloudWatch Metrics
- Includes CPU, Network, Disk, Status Check
- Basic Monitoring, collected at 5 minute interval
- Detailed Monitoring (paid), collected at a 1 minute interval

Custom metrics
- Basic resolution is 1 minute
- High resolution is 1 second
- Includes RAM and application level metrics
- IAM permissions on EC2 must be correct
- Use CloudWatch Agent (CloudWatch Monitoring scripts deprecated)

CPU Utilization, T Credit Use & Balance

Network In / Out

Status Check
- Instance status checks the EC2 VM
- System status checks the underlying hardware

Disk Read / Write Ops / Bytes Only for Instance Store

RAM is a custom metric, yours to push

CloudWatch Logs for EC2
- No logs go to CloudWatch by default
- Requires CloudWatch agent on EC2 to push log files
- CloudWatch log agent can also be run on-prem
- Requires IAM permissions to be correct

## :key: Lost SSH Key

EBS Backed:
- Stop instance, detach root volume
- Attach root volume to other instance as data volume
- Modify the ~/.ssh/authorized_keys file with your new key
- Move volume back to stopped instance
- Or using SSM: Run AWSSupport-ResetAccess document

Instance Store Backed:
- Can't stop instance or data is lost (AWS recommends termination)
- Use SSM to access via shell and edit authorized_keys file