# Auto Scaling Groups

Ensure minimum and maximum number of running instances

Scale out to match increased load

Scale in to match decreased load

Automatically register new instances to load balancer

## ASG Attributes

Launch configuration
- AMI + Instance type
- EC2 User Data
- EBS Volumes
- Security groups
- SSH Key Pair

Min size / Max size / Initial Capacity

Network & Subnet Information

Load Balancer Information

Scaling Policies

## Auto Scaling Alarms

Scale an ASG based on CloudWatch alarms

Monitor metrics such as Average CPU
- Computed for the overall ASG instances

## Auto Scaling Rules

- Target average CPU use
- Number of requests on ELB per instance
- Average network in/out

Custom Metrics:
- Send from app on EC2 to CloudWatch (PutMetric API)
- Create CloudWatch alarm to react to low/high values
- Use CloudWatch alarm as scaling policy for ASG
