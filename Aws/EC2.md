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
### Spot Instance
- Can get discount up to 90% compared to On-Demand
- Define a max spot price and keep the instance while current spot price < max:
- Hourly spot prices varies based on offer and capacity
- If current price > max, have 2 minutes to:
	- Stop instance (can be restart later)
	- Terminate instance
- Spot block:
	- Block instance during timeframe (1-6 hours)
	- In some case might still be claimed
- Use for batch jobs, data analysis, workloads that resilient to failure
- Not good for critical jobs and databases
- How to terminate spot instance:
	- ==First cancel a Spot Request, then terminate Instance associated with it==
	- Canceling Spot Request does not terminate Instance
	- Can only cancel Spot Instance requests that are **open, active, or disabled**
![[Pasted image 20240306002724.png]]

![[Pasted image 20240306002746.png]]
### Spot Fleets
- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
- Will try to meet target capacity with price constrains:
	- Define launch pools: instance type: (m5.large), OS, AZ
	- Can have multiple launch pools
	- Stop launching instances when reach capacity or max cost
- ==Strategy to allocate Spot Instances:==
	- lowest Price: from the pool with lowest price (cost optimization, short workloads)
	- diversified: distributed across all pools (great for availability, long workloads)
	- capacityOptimized: pool with optimal capacity for number of instances
	- priceCapacityOptimized (recommended): pools with highest capacity available, then select pool from that with lowest price (best for most workloads)
- Allow to automatically request Spot Instances with lowest prices
## Others
- [[IAM & AWS CLI#IAM Roles for Services|Instance role]]: provide access to AWS cli on EC2 instance
# Solution Architect Associate Level

## Private & Public  & Elastic IP
### Private vs Public
- 2 type:
	- IPv4
		- Most common
		- allows 3.7 billions different address
	- IPv6
		- Newer
		- Solve problems for IoT
### Elastic IP
- Stop and start EC2 can make it change its public IP
- If needed fixed public IP, you need Elastic IP
- Elastic IP is public IP you own as long as you don't delete
- Can be attached to 1 instance at a time
- Can mask the failure of an instance by remapping it to other working instance
- Can have 5 Elastic IP in your account (but can be increase)
- Try to ==avoid== Elastic IP:
	- Often reflect poor architecture
	- Instead, use a random public IP and register DNS name for it
	- Or using a Load Balancer and not use public IP (best pattern)
## Placement Groups
- Help control EC2 Instance Placement strategy
- Strategy:
	- Cluster: low latency group in a single AZ (high performance & high risk)
		- Great network
		- If the rack(hardware) fails, all instances fail
		- Use case:
			- Big Data job
			- Low latency & high throughput application
	- Spread: spread across hardware (max 7 instances per group per AZ), for critical applications
		- Pros:
			- Span across AZ
			- Reduce risk
			- Instances on different hardware
		- Cons:
			- Max 7 instances per AZ per placement group
		- Use:
			- App need high availability
			- Critical applications
	- Partitions: spread across many different partitions within an AZ. (scales to 100 instances per group) -> Hadoop, Cassandra, Kafka
		- Up to 7 partitions per AZ
		- Span multiple AZ
		- Up to 100s instance per group
		- Instances in different partition are safe from eachother
		- Can access partition information
		- Use case: Hadoop, Cassandra, Kafka
![[Pasted image 20240306234845.png]]
**Spread**
![[Pasted image 20240306235353.png]]
**Partition**
### Elastic Network Interface (ENI)
- Is a logical component in VPC that reprenses a virtual network card
- Consists of:
	- Private IPv4, 1 or more secondary IPv4
	- 1 Elastic IP (IPv4)  per private IPv4
	- 1 Public IP
	- 1 or more Security Groups
	- A MAC address
- Can be create independently and attach them on EC2 for failover
- Bound to 1 AZ


#ec2