# Fundamentals
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
## Elastic Network Interface (ENI)
- Is a logical component in VPC that reprenses a virtual network card
- Consists of:
	- Private IPv4, 1 or more secondary IPv4
	- 1 Elastic IP (IPv4)  per private IPv4
	- 1 Public IP
	- 1 or more Security Groups
	- A MAC address
- Can be create independently and attach them on EC2 for failover
- Bound to 1 AZ
## EC2 Hibernate
Instance state:
- Stop: data on disk (EBS) is kept intact
- Terminate: EBS volumes is lost
- Hibernate:
	- RAM state is preserved (is written to a file in the root EBS volume)
	- Instance boot will be faster
	- The root EBS volume must be encrypted
Use case:
- Long running services
- Saving RAM state
- Services take time to init
Good to know:
- RAM size < 150gb
- Not support bare metal instances
- Root volume must be EBS, encrypted, not instance store, large
- Available for OnDemand, Reserved and Spot instances
- Cannot be hibernated for more than 60 days

# Instance Storage
## EBS Overview
What's an EBS volume:
- EBS volume is a network drive attach to instances while running
- Allow instances to persist data, even after termination
- Can only mounted to one instances at a time (CCP level)
- Bound to specific AZ
- Analogy: "network USB stick"
EBS volume:
- Is a network drive:
	- Use network to communicate instance, can have latency
	- Can be detached and attached to others
- Locked to 1 AZ:
	- EBS from AZ 1 cannot be bound to AZ 2
	- To move volume across, need to snapshot
- Have a provisioned capacity (size in GBs, IOPS)
	- Get billed for all provisioned capacity
	- Can increase capacity over time
EBS delete on termination attribute (#ask_on_exam):
- Control  EBS behavior when instance terminates:
	- Default: root EBS is deleted
	- Default: other EBS is not deleted
- Can be controlled by AWS console/ CLI
- Use case: preserve root volume when instance is terminated
## EBS Snapshot
- Make backup of EBS volume at a point in time
- Not necessary to detach volume to snapshot but recommended
- Can copy across AZ or region
- Features:
	- Snapshot Archive:
		- 75% cheaper
		- take 24-72 hours to restoring the archive
	- Recycle Bin:
		- Setup rules to retain deleted snapshots so you can recover
		- Specify retention (1 day to 1 year)
	- Fast snapshot Restore (FSR):
		- Force full init to have no latency on first use (cost $)
## AMI Overview
- AMI = Amazon Machine Image
- Customizaton of EC2 instance:
	- Add your own software, config, OS, monitoring
	- Faster boot, config time because all software pre-packaged
- AMI are built for **specific region** (and can be copied across regions)
- Launch EC2 from:
	- Public AMI: Aws provided
	- Your own AMI: make and maintain yourself
	- AWS marketplace AMI: AMI someone else made (and can sell)
- Process:
	- Start EC2 and customize it
	- Stop EC2 
	- Build AMI - also create snapshot
	- Launch instances from other AMI
## EC2 Instance Store
- EBS volumes are network drives with good but limited performance
- If you need higher performance, use EC2 instance store
- Better I/O performance
- Will lose storage if stopped (ephemeral)
- Risk of data loss if hardware fails
- Manual backup & replicate
## EBS Volume Type
- EBS volume come in 6 types:
	- gp2/gp3 (SSD): general purpose SSD volume balances price/performance for wide variety of workloads
	- io1/io2 (SSD): highest performance SSD volume for mission-critical low-latency + high throughput workloads
	- st1 (HDD): low cost HDD volume designed for frequently access, throughput-intensive workloads
	- sc1 (HDD): lowest cost HDD volume designed for less frequently access workloads
- Characterized by Size | Throughput | IOPS
- Only gp2/gp3, io1/io2 can be used as boot volume
### General purpose SSD
- Cost effective- low latency
- System boot volumes, virtual desktops, development, test environments
- 1GiB - 16TiB
- gp3:
	- Baseline 3000 IOPS, throughput 125MiB/s
	- Can increase up to 16000 IOPS, throughput up to 1000MiB/s independently
- gp2:
	- Small gp2 volumes can burst IOPS to 3000
	- Size and IOPS are linked, max IOPS = 16000
	- 3 IOPS per GB, at 5334 GB -> at max IOPS
### Provisioned IOPS (PIOPS) SSD
- Critical business applications with sustained IOPS performance
- Application needs more than 16000 IOPS
- Great for database workload (sensitive to storage perf and consistency)
- io1/io2 (4GB - 16TB):
	- Max PIOPS: 64000 for nitro EC2, 32000 for other
	- Can increase PIOPS independently from storage size (like gp3)
	- io2 have more durability and more IOPS per GB (at the same price with io1)
- io2 Block Express (4GB-64TB):
	- Sub-millisecond latency
	- Max PIOPS: 256000 with an IOPS: GIBs ratio of 1000:1
- Support EBS multi-attach
### Hard Disk Drives (HDD)
- Cannot be boot volumes
- 125GB to 16TB
- Throughput optimized HDD (st1):
	- Big data, data warehouse, log processing
	- Max throughput 500MiB/s - max IOPs 500
- Cold HDD (st2):
	- For data that is infrequently accessed
	- Lowest cost
	- max throughput 250MiB/s - max IOPs 250
## EBS multi-attach
- For io1/io2 only
- Attach the same EBS volume  to multiple EC2 instances in the same AZ
- Each instance has full read/write permission to the volume
- Use case:
	- Achieve higher application availability in clustered Linux app (ex: Teradata)
	- App need manage concurrent write operations
- **Up to 16 EC2 instances at a time** (#ask_on_exam)
- Must use a file-system that's cluster-aware (not xfs, ext4 v..v)
- ![[Pasted image 20240403000407.png]]
## EBS encryption
- When you create at encrypted EBS volume, you get the following:
	- Data at rest is encrypted
	- All data in flight moving between instance and volume is encrypted
	- All snapshot are encrypted
	- All volume created from snapshot
- Encryption and decryption are handled transparently (you have nothing todo)
- Minimal impact on latency
- Leverages key from KMS (AES256)
- Copy unencrypted snapshot allow encryption
- Snapshot of encrypted volumes are also encrypted
- How to encryp an unencrypted EBS volume:
	- Create EBS snapshot
	- Encrypt using copy
	- Create new volume from snapshot (now will be encrypted)
	- Attach new volume to original instance
## Amazon EFS
- Managed NFS (network file system) that can be mounted on many EC2
- EFS works with EC2 instances in multiple-AZ
- Highly available, scalable, expensive (3x gp2), pay per use
- Use case: content-management, web serving, data sharing, wordpress
- Use NFSv4.1 protocol
- Use security group to control EFS
- Only compatible with Linux (not windows)
- Encrypted at rest using KMS
- POSIX file system (Linux) that has standard API
- File system scale automatically, pay per use
- Performance & Storage:
	- EFS scale:
		- 1000s concurent NFS client, 10GB+/s throughput
		- Grow to Petabyte-scale network system, automatically
	- Performance mode: (set at EFS creation time)
		- General purpose (default): latency-sensitive use cases (webserver, CMS)
		- Max I/O: higher latency, throughput, highly parallel (big data, media processing)
	- Throughput Mode:
		- Bursting - 1TB = 50MiB/s + burst up to 100MiB/s
		- Provisioned - set throughput regardless of storage size: ex: 1GB/s for 1 TB storage
		- Elastic: auto scale throughput up and down based on workloads
			- Use for unpredicted workload
- Storage classes:
	- Storage tiers:
		- Standard: frequently accessed files
		- Infrequent access (EFS-IA): cost to retrieve files, lower price to store. Enable using Lifecycle policy
	- Availability and durability:
		- Standard: multi-AZ, great for prod
		- One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS OneZone-IA)
		- Over 90% in cost savings
## EFS vs EBS
- EBS volumes:
	- 1 instance (except multi attach io1/ io2)
	- Are locked at AZ level
	- gp2: IO increase if disk size increase
	- io1: can increase IO indefinitely
- To migrate EBS across AZ:
	- Take a snapshot
	- Restore snapshot to another AZ
	- EBS backup use IO so shouldn't run when application handling a lot of traffic
- Root EBS Volumes of instance will get terminated if the instance is terminated by default (but can be changed)
- EFS:
	- Mounting 100s of instance across AZ
	- EFS share website files (WordPress)
	- Only for Linux (POSIX)
	- Higher price point than EBS
	- Can leverage EFS-IA for cost saving
#ec2