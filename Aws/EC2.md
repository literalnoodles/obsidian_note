# Introduction
Basic:
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
- EC2 Instance Type:
	- Naming: m5.2xlarge
		- m -> Instance class
		- 5 -> generation
		- 2xlarge -> size
	- Type:
		- General purpose (t2 ...)
		- Computer Optimized: tasks require high CPU (c5, c6 ...)
		- Memory optimized: process large data set in memory (r5, r6)
		- Storage optimized: read & write large dataset to local storage

#ec2