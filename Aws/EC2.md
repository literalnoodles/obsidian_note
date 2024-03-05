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
## Purchasing options
- **[[#Spot Instances|Spot Instances]]**: short workload, cheap, can lose instances
- **[[#EC2 On-Demand|On-Demand]] Instances**: short workload, predictable pricing, pay by second
- **[[#Reserved Instances|Reserved]]** (1 & 3 years):
	- Reserved Instances: long workloads
	- Convertible Reserved Instances: long workloads & flexible instances
- **[[#Saving plans|Saving Plans]]** (1 & 3 years):
	- Long workloads, commit to an amount of usage
- **[[#Dedicated Host|Dedicated Hosts]]**: book an entire physical server, control instance placement
- **[[#Dedicated Instances|Dedicated Instances]]**: no other customers will share your hardware
- **Capacity Reservations**: reserve capacity in a specific AZ for any duration
### EC2 On-Demand

- Pay for what you use:
	- Linux & Win: billing per second, after first minute
	- Other: billing per hour
- Highest cost but no upfront payment
- No long-term commitment
- -> ==For short-term & un-interrupted workloads==
### Reserved Instances
- Up to 72% discount compare to On-Demand
- Reserve specific attributes (Instance type, Region, Tenancy, OS)
- Period:
	- 1 year (+ discount)
	- 3 years (+++ discount)
- Payment options:
	- No upfront (+)
	- Partial upfront (++)
	- All Upfront (+++)
- Reserved Instance's scope (Region or Zonal) (reserve capacity in a specific AZ)
- Can buy or sell in Reserved Instance Marketplace
- -> ==For steady-state usage applications (database...)==
- ==Convertible Instance==:
	- Can change Instance type, Instance family, OS, Scope and tenancy
	- Less discount (up to 66%)
### Saving plans
- Discount on long-term usage (up to 72%)
- Commit to certain type of usage (10$/hour for 1 or 3 years)
- Usage beyond is billed at On-Demand price
- Locked to specific ==Instance Family & Region== (Eg: M5 in us-east-1)
- Flexible:
	- ==Instance size== (xlarge, 2xlarge)
	- ==OS== (Linux, Window)
	- ==Tenancy== (Host, Dedicated, Default)
### Spot Instances
- Up to 90% discount compare to On-Demand
- Can lose instance anytime if max price < spot price
- MOST cost-efficient instances in AWS
- -> ==For workloads that resilient to failure==:
	- Batch job
	- Data analysis
	- Image processing
	- Distributed workload
	- Flexible start and end time
- -> ==Not suitable for critical jobs or databases==
### Dedicated Host
- Physical server with EC2
- Allow
	- Compliance requirements
	- Use existing server-bound software licenses (per-socket, per-core, per-VM)
- Purchasing options:
	- On-Demand: pay per second for active Dedicated Host
	- Reserved: 1 or 3 year (No, Partial, All Upfront)
- Most expensive
- -> ==Use for software with complicated licensing model==
- -> ==Use for companies have strong regulatory or compliance needs ==
### Dedicated Instances
- Instance run on hardware dedicated to you
- May share hardware with other instances in the same account
- No control over instance placement (can move hardware after stop/start)

|                                       | Dedicated Instances | Dedicated Hosts |
| ------------------------------------- | ------------------- | --------------- |
| Dedicated Server                      | x                   | x               |
| Per instance billing                  | x                   |                 |
| Per host billing                      |                     | x               |
| Visible of socket, core, host ID      |                     | x               |
| Affinity between host and instance    |                     | x               |
| Targeted instance placement           |                     | x               |
| Automatic instance placement          | x                   | x               |
| Add capacity using allocation request |                     | x               |
### Capacity Reservation
- Reserve **On-Demand** Instance Capacity in a specific AZ for any duration
- Always have access to EC2 capacity when needed
- **No time commitment** (create/cancel anytime), **no billing discount**
- Combine with Regional Reserved Instances & Saving Plans for billing discount
- Charged at On-Demand rate whether you run instance or not
- ==-> For short-term, uninterrupted workloads in a specific AZ==
### Which options?
![[Pasted image 20240305214325.png]]
## EC2 Spot Instance & Spot Fleet

## Others
- [[IAM & AWS CLI#IAM Roles for Services|Instance role]]: provide access to AWS cli on EC2 instance

#ec2