# Introduction
---
## Basic
- EC2 = Elastic Compute Cloud
- Capability:
	- Renting virtual machines (EC2)
	- Storing data on virtual drive (EBS)
	- Distribute load (ELB)
	- Scaling using auto-scaling group (ASG)
- Configure:
	- OS: Linux, Windows, Mac
	- CPU
	- RAM
	- Storage
	- Network card: Speed, Public IP
	- Firewall rules: security group
	- Bootstrap script: EC2 user data
- EC2 User Data:
	- run only one at first start
	- run as root user
---
## EC2 Instance Type
- Naming: m5.2xlarge
	- m -> Instance class
	- 5 -> generation
	- 2xlarge -> size
- Type:
	- General purpose (t2 ...)
	- Computer Optimized: tasks require high CPU (c5, c6 ...)
	- Memory optimized: process large data set in memory (r5, r6)
	- Storage optimized: read & write large dataset to local storage
---
## Security groups
- Acting as firewall
- Control traffic in and out of EC2
- Only contains ==**allow**== rule
- Rules can be reference by IP or by security group
- Regulate:
	- Access to ports
	- Authorized IP ranges
	- Inbound network
	- Outbound network
- Can be attached to multiple instances
- Locked down to a region/vpc
- If block, ec2 instance won't see the traffic
- By default:
	- Inbound is blocked
	- Outbound is authorised
- Referencing
![[Pasted image 20240304211530.png]]
---
## Others
- [[IAM & AWS CLI#IAM Roles for Services|Instance role]]: provice access to aws cli on EC2 instance

#ec2