# My SAA-C02 Notes
> These are my personal notes from AWS Documentation and a Udemy course.
I would highly recommend this course by Stephane
[aws-sa-associate-saac02](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/).

## Table of Contents

- [AWS-Infrastructure](#AWS-Infrastructure)
- [IAM-Accounts-Organizations](#IAM-Accounts-Organizations)
- [Amazon-S3](#Amazon-S3)
- [VPC](#VPC)
- [EC2](#EC2)
- [Containers](#Containers)
- [Additional-EC2-Info](#Additional-EC2-Info)
- [Route53](#Route53)
- [Amazon-RDS](#Amazon-RDS)
- [Network-Storage-EFS](#Network-Storage-EFS)
- [HA-Scale](#HA-Scale)
- [Serverless-Services](#Serverless-Services)
- [CDN-Network-Optimization](#CDN-Network-Optimization)
- [Other-VPC-Topics](#Other-VPC-Topics)
- [Migration](#Migration)
- [Security-Operations](#Security-Operations)
- [DynamoDB](#DynamoDB)

---

## AWS-Infrastructure

### [Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/?p=ngi&loc=1)

#### Regions

AWS has the concept of a Region, which is a physical location around the world where we cluster data centers. We call each group of logical data centers an Availability Zone. Each AWS Region consists of multiple, isolated, and physically separate AZ's within a geographic area. Each AZ has independent power, cooling, and physical security and is connected via redundant, ultra-low-latency networks. Note, a region is simply a cluster of datacenters where most AWS services are region-scoped.

Names can be us-east-1 or eu-west-3 and include areas such as countries or states

- Texas
- North America
- California
- Singapore
- Beijing
- London
- Paris
- Tokyo

AWS maintains multiple geographic Regions, including Regions in North America, South America, Europe, China, Asia Pacific, South Africa, and the Middle East. To view a region near you or your users [click here](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/?p=ngi&loc=4).

#### Availability Zones

An Availability Zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity in an AWS Region. All AZ’s in an AWS Region are interconnected with high-bandwidth, low-latency networking, over fully redundant, dedicated metro fiber providing high-throughput, low-latency networking between AZ’s. All traffic between AZ’s is encrypted. AZ’s are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles) of each other.

Each region has many AZ's, usually 3 AZ's in each region (minimum of 2, maximum of 6).

#### Benefits of Regions

- Security
- Availability
- Performance
- Global Footprint
- Scalability
- Flexibility

#### Local Zones

AWS Local Zones are a new type of AWS infrastructure deployment that places AWS compute, storage, database, and other select services closer to large population, industry, and IT centers where no AWS Region exists today. You can easily run latency-sensitive portions of applications local to end-users and resources in a specific geography, delivering single-digit millisecond latency. Each AWS Local Zone location is an extension of an AWS Region where you can run your latency-sensitive applications. Local Zones are managed and supported by AWS. 

Use Cases: Media & Entertainment Content Creation, Real-time Multiplayer Gaming, Reservoir Simulations, Electronic Design Automation, and Machine Learning.

---

## IAM-Accounts-Organizations

### AWS IAM

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources. 

User
- An AWS Identity and Access Management (IAM) user is an entity that you create in AWS to represent the person or application that uses it to interact with AWS. A user in AWS consists of a name and credentials.

Principals
- A person or application that uses the AWS account root user, an IAM user, or an IAM role to sign in and make requests to AWS.

Role
- An IAM identity that you can create in your account that has specific permissions. An IAM role has some similarities to an IAM user. Roles and users are both AWS identities with permissions policies that determine what the identity can and cannot do in AWS.
- Use IAM to configure roles when using CLI within AWS
- Roles can be attached to EC2 instances
- For example, when making secure calls from EC2 to S3 buckets, create an IAM role granting least privilege and assign it to the EC2 instance

Federation
- The creation of a trust relationship between an external identity provider and AWS. Users can sign in to a web identity provider, such as Login with Amazon, Facebook, Google, or any IdP that is compatible with OpenID Connect (OIDC). Users can also sign in to an enterprise identity system that is compatible with Security Assertion Markup Language (SAML) 2.0, such as Microsoft Active Directory Federation Services. 

Permissions boundary
- An advanced feature in which you use policies to limit the maximum permissions that an identity-based policy can grant to a role. You cannot apply a permissions boundary to a service-linked role.
- Use a managed policy as the permissions boundary for an IAM entity (user or role). That policy defines the maximum permissions that the identity-based policies can grant to an entity, but does not grant permissions. Permissions boundaries do not define the maximum permissions that a resource-based policy can grant to an entity.

Organizations SCP
- Use an AWS Organizations service control policy (SCP) to define the maximum permissions for account members of an organization or organizational unit (OU). SCPs limit permissions that identity-based policies or resource-based policies grant to entities (users or roles) within the account, but do not grant permissions.
- SCPs are JSON policies that specify the maximum permissions for an organization or organizational unit (OU). The SCP limits permissions for entities in member accounts, including each AWS account root user. An explicit deny in any of these policies overrides the allow.

#### Inline Policies and Managed Policies

- Inline Policy: Policies that you create and manage and that are embedded directly into a single user, group, or role. In most cases, we don't recommend using inline policies.
- Managed Policy: Standalone identity-based policies that you can attach to multiple users, groups, and roles in your AWS account. You can use two types of managed policies: AWS managed policies and customer managed policies.

#### Amazon Resource Name (ARN)

Amazon Resource Names (ARNs) uniquely identify AWS resources. We require an ARN when you need to specify a resource unambiguously across all of AWS, such as in IAM policies, Amazon Relational Database Service (Amazon RDS) tags, and API calls.

ARN [format](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html):

```bash
arn:partition:service:region:account-id:resource-id
arn:partition:service:region:account-id:resource-type/resource-id
arn:partition:service:region:account-id:resource-type:resource-id
```

### AWS Organizations

AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage. AWS Organizations includes account management and consolidated billing capabilities that enable you to better meet the budgetary, security, and compliance needs of your business. As an administrator of an organization, you can create accounts in your organization and invite existing accounts to join the organization.

- Centralized management of all of your AWS accounts
- Consolidated billing for all member accounts
- Hierarchical grouping of your accounts to meet your budgetary, security, or compliance needs
- Policies to centralize control over the AWS services and API actions that each account can access
- Policies to standardize tags across the resources in your organization's accounts
- Policies to control how AWS artificial intelligence (AI) and machine learning services can collect and store data.
- Integration and support for AWS Identity and Access Management (IAM)
- Integration with other AWS services
- Data replication that is eventually consistent

#### Organization Root

The parent container for all the accounts for your organization. If you apply a policy to the root, it applies to all organizational units (OUs) and accounts in the organization.

### Service Control Policies

Service control policies (SCPs) are a type of organization policy that you can use to manage permissions in your organization. SCPs offer central control over the maximum available permissions for all accounts in your organization. SCPs help you to ensure your accounts stay within your organization’s access control guidelines. 

SCPs are similar to AWS Identity and Access Management (IAM) permission policies and use almost the same syntax. However, an SCP never grants permissions. Instead, SCPs are JSON policies that specify the maximum permissions for an organization or organizational unit (OU). 
- SCPs affect only principals that are managed by accounts that are part of the organization.
- An SCP restricts permissions for principals in member accounts, including each AWS account root user.
- SCPs do not affect any service-linked role.


---

## Amazon-S3

### S3 Overview

Amazon S3 has a simple web services interface that you can use to store and retrieve any amount of data, at any time, from anywhere on the web.

Buckets
- A bucket is a container for objects stored in Amazon S3. Every object is contained in a bucket. 

Objects
- Objects are the fundamental entities stored in Amazon S3. Objects consist of object data and metadata. 

Keys
- A key is the unique identifier for an object within a bucket. Every object in a bucket has exactly one key. The combination of a bucket, key, and version ID uniquely identify each object. 

Amazon S3 data consistency model
- Amazon S3 provides read-after-write consistency for PUTS of new objects in your S3 bucket in all Regions with one caveat. The caveat is that if you make a HEAD or GET request to a key name before the object is created, then create the object shortly after that, a subsequent GET might not return the object due to eventual consistency.
- S3 offers eventual consistency for overwrite PUTS and DELETES in all Regions

#### Bucket Policies

Bucket policies provide centralized access control to buckets and objects based on a variety of conditions, including Amazon S3 operations, requesters, resources, and aspects of the request (for example, IP address). The policies are expressed in the access policy language and enable centralized management of permissions. The permissions attached to a bucket apply to all of the objects in that bucket.
- Unlike access control lists (described later), which can add (grant) permissions only on individual objects, policies can either add or deny permissions across all (or a subset) of objects within a bucket.

### Hosting a static website using S3

You can use Amazon S3 to host a static website. On a static website, individual webpages include static content. They might also contain client-side scripts.
- To host a static website on Amazon S3, you configure an Amazon S3 bucket for website hosting and then upload your website content to the bucket. When you configure a bucket as a static website, you must enable website hosting, set permissions, and create and add an index document.
- You can then access the bucket through the AWS Region-specific Amazon S3 website endpoints for your bucket
- S3 doesn't support HTTPS access for website endpoints (use CloudFront instead)

### S3 Object Versioning and MFA Delete

Versioning is a means of keeping multiple variants of an object in the same bucket. You can use versioning to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket. With versioning, you can easily recover from both unintended user actions and application failures. When you enable versioning for a bucket, if Amazon S3 receives multiple write requests for the same object simultaneously, it stores all of the objects.
- Versioning-enabled buckets enable you to recover objects from accidental deletion or overwrite.
- The versioning state applies to all (never some) of the objects in that bucket.

#### MFA Delete

You can optionally add another layer of security by configuring a bucket to enable MFA (multi-factor authentication) Delete, which requires additional authentication for either of the following operations:
- Change the versioning state of your bucket
- Permanently delete an object version

#### S3 Transfer Acceleration

Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket. Transfer Acceleration takes advantage of Amazon CloudFront’s globally distributed edge locations. As the data arrives at an edge location, data is routed to Amazon S3 over an optimized network path.

Use Cases
- You have customers that upload to a centralized bucket from all over the world
- You transfer gigabytes to terabytes of data on a regular basis across continents
- You are unable to utilize all of your available bandwidth over the Internet when uploading to Amazon S3

### S3 Encryption

#### Types

Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)
- When you use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3), each object is encrypted with a unique key. As an additional safeguard, it encrypts the key itself with a master key that it regularly rotates. Amazon S3 server-side encryption uses one of the strongest block ciphers available, 256-bit Advanced Encryption Standard (AES-256), to encrypt your data.
- AWS owned and managed keys (manages encryption and decryption processs automatically)

Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS)
- Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS) is similar to SSE-S3, but with some additional benefits and charges for using this service. There are separate permissions for the use of a CMK that provides added protection against unauthorized access of your objects in Amazon S3. SSE-KMS also provides you with an audit trail that shows when your CMK was used and by whom. Additionally, you can create and manage customer managed CMKs or use AWS managed CMKs that are unique to you, your service, and your Region.
- encryption keys are stored by organization
- you have control of rotation policy

Server-Side Encryption with Customer-Provided Keys (SSE-C)
- With Server-Side Encryption with Customer-Provided Keys (SSE-C), you manage the encryption keys and Amazon S3 manages the encryption, as it writes to disks, and decryption, when you access your objects.
- encryption keys are stored by organization

Client-Side Encryption
- Encrypt data client-side and upload the encrypted data to Amazon S3. In this case, you manage the encryption process, the encryption keys, and related tools.
- Objects are encrypted by client before they are sent to AWS

### S3 Object Storage Classes

#### S3 Standard

- The default storage class. If you don't specify the storage class when you upload an object, Amazon S3 assigns the S3 Standard storage class.
- instantaneous retrieval time
- no default storage day minimum
- can be stored as little as 24 hours or more
- use cases: big data, mobile, gaming data

#### S3 Intelligent-Tiering

- designed to optimize storage costs by automatically moving data to the most cost-effective storage access tier, without performance impact or operational overhead
- Intelligent-Tiering storage class is ideal if you want to optimize storage costs automatically for long-lived data when access patterns are unknown or unpredictable
- suitable for objects larger than 128 KB that you plan to store for at least 30 days

Designed for data that isn't accessed often, long term storage, backups,
disaster recovery files. The requirement for data to be safe is most important.

#### Standard-IA and One Zone-IA

- S3 Standard-IA and S3 One Zone-IA storage classes are designed for long-lived and infrequently accessed data
- instantaneous retrieval time
- objects are available for millisecond access
- use cases: storing backups and older data that is accessed infrequently, but that still requires millisecond access
- One Zone-IA stores the object data in only one Availability Zone, which makes it less expensive than S3 Standard-IA
  - Use if you can re-create the data if the Availability Zone fails

#### S3 Glacier

- Use for archives where portions of the data might need to be retrieved in minutes
- Data stored in the S3 Glacier storage class has a minimum storage duration period of 90 days and can be accessed in as little as 1-5 minutes using expedited retrieval

Retrieval methods:
- Expedited: 1 - 5 minutes (very expensive)
- Standard: 3 - 5 hours to restore
- Bulk: 5 - 12 hours (least expensive)

#### S3 Glacier Deep Archive

- Use for archiving data that rarely needs to be accessed
- Data stored in the S3 Glacier Deep Archive storage class has a minimum storage duration period of 180 days and a default retrieval time of 12 hours

Retrieval methods:
- Standard (default): 12 hours
- Bulk: 48 hours

#### S3 Lifecycle Management

To manage your objects so that they are stored cost effectively throughout their lifecycle, configure their Amazon S3 Lifecycle.
- Transition actions — Define when objects transition to another storage class. For example, you might choose to transition objects to the S3 Standard-IA storage class 30 days after you created them, or archive objects to the S3 Glacier storage class one year after creating them.
- Expiration actions — Define when objects expire. Amazon S3 deletes expired objects on your behalf.

Use Cases:
- If you upload periodic logs to a bucket, your application might need them for a week or a month. After that, you might want to delete them.
- Some documents are frequently accessed for a limited period of time. After that, they are infrequently accessed. At some point, you might not need real-time access to them, but your organization or regulations might require you to archive them for a specific period. After that, you can delete them.
- You might upload some types of data to Amazon S3 primarily for archival purposes. For example, you might archive digital media, financial and healthcare records, raw genomics sequence data, long-term database backups, and data that must be retained for regulatory compliance.

### S3 Replication

Replication enables automatic, asynchronous copying of objects across Amazon S3 buckets. Buckets that are configured for object replication can be owned by the same AWS account or by different accounts. You can copy objects between different AWS Regions or within the same Region.

Use Cases:
- Replicate objects while retaining metadata
- Replicate objects into different storage classes
- Maintain object copies under different ownership
- Replicate objects within 15 minutes

#### Types of Object Replication

1. Cross-Region replication (CRR) is used to copy objects across Amazon S3 buckets in different AWS Regions.
- Meet compliance requirements
- Minimize latency and response times
- Increase operational efficiency

2. Same-Region replication (SRR) is used to copy objects across Amazon S3 buckets in the same AWS Region.
- Aggregate logs into a single S3 bucket
- Configure live replication between production and test accounts
- Abide by data sovereignty laws

### S3 Presigned URL

- All objects by default are private. Only the object owner has permission to access these objects. However, the object owner can optionally share objects with others by creating a presigned URL, using their own security credentials, to grant time-limited permission to download the objects.

- When you create a presigned URL for your object, you must provide your security credentials, specify a bucket name, an object key, specify the HTTP method (GET to download the object) and expiration date and time. The presigned URLs are valid only for the specified duration.

- Anyone who receives the presigned URL can then access the object. For example, if you have a video in your bucket and both the bucket and the object are private, you can share the video with others by generating a presigned URL.

---

## VPC

### Networking Refresher

#### Private IP Addressing

These IP's cannot communicate with the internet directly.

- 1 Class A network: `10.0.0.0` - `10.255.255.255`
- 16 Class B networks: `172.16.0.0` - `172.31.255.255`
- 256 Class C networks: `192.168.0.0` - `192.168.255.255`

You can have multiple VPC's in a region (max = 5)

Amazon VPC supports IPv4 and IPv6 addressing, and has different CIDR block size quotas for each. By default, all VPCs and subnets must have IPv4 CIDR blocks—you can't change this behavior. You can optionally associate an IPv6 CIDR block with your VPC.

MAx CIDR per VPC is 5 but for each VPC:
- min size is /28 = 16 IP addresses
- max size / 16 = 65536 IP addresses

For example, if CIDR block 10.0.0.0/24, reserved IP are:
- 10.0.0.0: Network address
- 10.0.0.1: Reserved by AWS for the VPC router
- 10.0.0.2: Reserved by AWS for mapping to Amazon-provided DNS
- 10.0.0.3: Reserved by AWS for future use
- 10.0.0.255: Network broadcast address. AWS does not support broadcast in a VPC, therefore the address is reserved.

#### Internet Gateway
- Your default VPC includes an internet gateway, and each default subnet is a public subnet.
- Each instance that you launch into a default subnet has a private IPv4 address and a public IPv4 address. These instances can communicate with the internet through the internet gateway. An internet gateway enables your instances to connect to the internet through the Amazon EC2 network edge.
- You can enable internet access for an instance launched into a nondefault subnet by attaching an internet gateway to its VPC (if its VPC is not a default VPC) and associating an Elastic IP address with the instance.

#### Bastion Host

Including bastion hosts in your VPC environment enables you to securely connect to your Linux instances without exposing your environment to the Internet. After you set up your bastion hosts, you can access the other instances in your VPC through Secure Shell (SSH) connections on Linux. Bastion hosts are also configured with security groups to provide fine-grained ingress control.

Best practices when implementing bastion hosts:
- When you add new instances to the VPC that require management access from the bastion host, make sure to associate a security group ingress rule, which references the bastion security group as the source, with each instance. It is also important to limit this access to the required ports for administration.
- During deployment, the public key from the selected Amazon EC2 key pair is associated with the user ec2-user in the Linux instance. For additional users, you should create users with the required permissions and associate them with their individual authorized public keys for SSH connectivity.
- For the bastion host instances, you should select the number and type of instances according to the number of users and operations to be performed. The Quick Start creates one bastion host instance and uses the t2.micro instance type by default, but you can change these settings during deployment.
- Set your desired expiration time directly in the CloudWatch Logs log group for the logs collected from each bastion instance. This ensures that bastion log history is retained only for the amount of time you need.
- Keep your CloudWatch log files separated for each bastion host restarting the instance so that you can filter and isolate logs messages from individual bastion hosts more easily. Every instance that is launched by the bastion Auto Scaling group will create its own log stream based on the instance ID.

### Network Access Control List (NACL)

A network access control list (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. You might set up network ACLs with rules similar to your security groups in order to add an additional layer of security to your VPC.

NACL basics:
- Your VPC automatically comes with a modifiable default network ACL. By default, it allows all inbound and outbound IPv4 traffic and, if applicable, IPv6 traffic.
- You can create a custom network ACL and associate it with a subnet. By default, each custom network ACL denies all inbound and outbound traffic until you add rules.
- Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.
- You can associate a network ACL with multiple subnets. However, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed.
- A network ACL contains a numbered list of rules. We evaluate the rules in order, starting with the lowest numbered rule, to determine whether traffic is allowed in or out of any subnet associated with the network ACL. The highest number that you can use for a rule is 32766. We recommend that you start by creating rules in increments (for example, increments of 10 or 100) so that you can insert new rules where you need to later on.
- A network ACL has separate inbound and outbound rules, and each rule can either allow or deny traffic.
- Network ACLs are stateless, which means that responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).

### Security Groups

- A security group acts as a virtual firewall for your instance to control inbound and outbound traffic. When you launch an instance in a VPC, you can assign up to five security groups to the instance. Security groups act at the instance level, not the subnet level. Therefore, each instance in a subnet in your VPC can be assigned to a different set of security groups.
- If you launch an instance using the Amazon EC2 API or a command line tool and you don't specify a security group, the instance is automatically assigned to the default security group for the VPC. If you launch an instance using the Amazon EC2 console, you have an option to create a new security group for the instance.
- For each security group, you add rules that control the inbound traffic to instances, and a separate set of rules that control the outbound traffic. This section describes the basic things that you need to know about security groups for your VPC and their rules.

#### Security Group vs NACL

- Security groups are stateful: any changes applied to an incoming rule will be automatically applied to the outgoing rule
- NACLs are stateless: any changes applied to an incoming rule will not be applied to the outgoing rule
- Security group support allow rules only (by default all rules are denied)
- NACLs support allow and deny rules
- All rules in a security group are applied whereas rules are applied in their order (the rule with the lower number gets processed first) in Network ACL
- Security group first layer of defense, whereas Network ACL is second layer of the defense

### NAT Gateway

- enables instances in a private subnet to connect to the internet or other AWS services, but prevent the internet from initiating a connection with those instances
- You are charged for creating and using a NAT gateway in your account. NAT gateway hourly usage and data processing rates apply.
- do not support IPv6 (use egress-only gateway instead)
- Each NAT gateway is created in a specific Availability Zone and implemented with redundancy in that zone.
- supports 5 Gbps of bandwidth and automatically scales up to 45 Gbps
- supports TCP, UDP, and ICMP
- cannot associate a security group with a NAT gateway
- You can use a network ACL to control the traffic to and from the subnet in which the NAT gateway is located.
- use NAT Gateway over NAT Instance

### VPC Peering

A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them privately. Instances in either VPC can communicate with each other as if they are within the same network. You can create a VPC peering connection between your own VPCs, with a VPC in another AWS account, or with a VPC in a different AWS Region.

A VPC peering connection helps you to facilitate the transfer of data.

You can establish peering relationships between VPCs across different AWS Regions (also called Inter-Region VPC Peering). This allows VPC resources including EC2 instances, Amazon RDS databases and Lambda functions that run in different AWS Regions to communicate with each other using private IP addresses, without requiring gateways, VPN connections, or separate network appliances. The traffic remains in the private IP space. All inter-region traffic is encrypted with no single point of failure, or bandwidth bottleneck. Traffic always stays on the global AWS backbone, and never traverses the public internet, which reduces threats, such as common exploits, and DDoS attacks. Inter-Region VPC Peering provides a simple and cost-effective way to share resources between regions or replicate data for geographic redundancy.

---

## EC2

Amazon Elastic Compute Cloud (Amazon EC2) provides scalable computing capacity in the AWS cloud. Simply put, it's IaaS.

### EC2 Architecture and Resilience

EC2 instances are virtual machines running on Amazon hardware (physical hosts).

- Virtual computing environments, known as instances
- Preconfigured templates for your instances, known as Amazon Machine Images (AMIs), that package the bits you need for your server (including the operating system and additional software)
- Various configurations of CPU, memory, storage, and networking capacity for your instances, known as instance types
- Secure login information for your instances using key pairs (AWS stores the public key, and you store the private key in a secure place)
- Storage volumes for temporary data that's deleted when you stop or terminate your instance, known as instance store volumes
- Persistent storage volumes for your data using Amazon Elastic Block Store (Amazon EBS), known as Amazon EBS volumes
- Multiple physical locations for your resources, such as instances and Amazon EBS volumes, known as Regions and Availability Zones
- A firewall that enables you to specify the protocols, ports, and source IP ranges that can reach your instances using security groups
- Static IPv4 addresses for dynamic cloud computing, known as Elastic IP addresses
- Metadata, known as tags, that you can create and assign to your Amazon EC2 resources
- Virtual networks you can create that are logically isolated from the rest of the AWS cloud, and that you can optionally connect to your own network, known as virtual private clouds (VPCs)

EC2 Networking (ENI)

An elastic network interface is a logical networking component in a VPC that represents a virtual network card. You can create a network interface, attach it to an instance, detach it from an instance, and attach it to another instance. Every instance in a VPC has a default network interface, called the primary network interface. You cannot detach a primary network interface from an instance. You can create and attach additional network interfaces. 

### EC2 Instance Types

- Use this **awesome** tool: https://ec2instances.info/

A1	General purpose\
C4	Compute optimized\
C5	Compute optimized\
C5a	Compute optimized\
C5d	Compute optimized\
C5n	Compute optimized\
C6g	Compute optimized\
D2	Storage optimized\
F1	Accelerated computing\
G3	Accelerated computing\
G4	Accelerated computing\
H1	Storage optimized\
I3	Storage optimized\
I3en	Storage optimized\
Inf1	Accelerated computing\
M4	General purpose\
M5	General purpose\
M5a	General purpose\
M5ad	General purpose\
M5d	General purpose\
M5dn	General purpose\
M5n	General purpose\
M6g	General purpose\
P2	Accelerated computing\
P3	Accelerated computing\
P3dn	Accelerated computing\
R4	Memory optimized\
R5	Memory optimized\
R5a	Memory optimized\
R5ad	Memory optimized\
R5d	Memory optimized\
R5dn	Memory optimized\
R5n	Memory optimized\
R6g	Memory optimized\
T2	General purpose\
T3	General purpose\
T3a	General purpose\
u-xtb1	Memory optimized\
X1	Memory optimized\
X1e	Memory optimized\
z1d	Memory optimized\

## EC2 Storage

### Elastic Block Store (EBS)

Amazon Elastic Block Store (Amazon EBS) provides block level storage volumes for use with EC2 instances. EBS volumes behave like raw, unformatted block devices. You can mount these volumes as devices on your instances. EBS volumes that are attached to an instance are exposed as storage volumes that persist independently from the life of the instance. You can create a file system on top of these volumes, or use them in any way you would use a block device (such as a hard drive). You can dynamically change the configuration of a volume attached to an instance.
- EBS volumes are created in a specific Availability Zone, and can then be attached to any instances in that same Availability Zone. To make a volume available outside of the Availability Zone, you can create a snapshot and restore that snapshot to a new volume anywhere in that Region. You can copy snapshots to other Regions and then restore them to new volumes there, making it easier to leverage multiple AWS Regions for geographical expansion, data center migration, and disaster recovery.

#### General Purpose SSD (gp2)

General Purpose SSD volumes offer a base performance of 3 IOPS/GiB, with the ability to burst to 3,000 IOPS for extended periods of time. These volumes are ideal for a broad range of use cases such as boot volumes, small and medium-size databases, and development and test environments.
- virtual desktop (VDI) environments
- can be a boot volume
- for low latency apps

#### Provisioned IOPS SSD (io1)

Provisioned IOPS SSD volumes support up to 64,000 IOPS and 1,000 MiB/s of throughput. This allows you to predictably scale to tens of thousands of IOPS per EC2 instance.
- can be a boot volume
- write-heavy, updating moreso than reading data
- highest performance
- for large critical databases

#### Throughput Optimized (st1)

Throughput Optimized HDD volumes provide low-cost magnetic storage that defines performance in terms of throughput rather than IOPS. These volumes are ideal for large, sequential workloads such as Amazon EMR, ETL, data warehouses, and log processing.
- big data apps
- streaming workloads
- data warehouse apps

#### Cold HDD (sc1)

Cold HDD volumes provide low-cost magnetic storage that defines performance in terms of throughput rather than IOPS. These volumes are ideal for large, sequential, cold-data workloads. If you require infrequent access to your data and are looking to save costs, these volumes provides inexpensive block storage. 
- lowest cost storage

### EC2 Instance Store

An instance store provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer. Instance store is ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers.
- You can specify instance store volumes for an instance only when you launch it. You can't detach an instance store volume from one instance and attach it to a different instance.
- The data in an instance store persists only during the lifetime of its associated instance. If an instance reboots (intentionally or unintentionally), data in the instance store persists. However, data in the instance store is lost under any of the following circumstances:
  - The underlying disk drive fails
  - The instance stops
  - The instance terminates
- Therefore, do not rely on instance store for valuable, long-term data. Instead, use more durable data storage, such as Amazon S3, Amazon EBS, or Amazon EFS.
- When you stop or terminate an instance, every block of storage in the instance store is reset. Therefore, your data cannot be accessed through the instance store of another instance.
- **Instance Store have very high IOPS with millions of IOPS (read and write)**

### EBS Snapshots

You can back up the data on your Amazon EBS volumes to Amazon S3 by taking point-in-time snapshots. Snapshots are incremental backups, which means that only the blocks on the device that have changed after your most recent snapshot are saved. This minimizes the time required to create the snapshot and saves on storage costs by not duplicating data. When you delete a snapshot, only the data unique to that snapshot is removed. Each snapshot contains all of the information that is needed to restore your data (from the moment when the snapshot was taken) to a new EBS volume.

- EBS snapshots fully support EBS encryption.
- Snapshots of encrypted volumes are automatically encrypted.
- Volumes that you create from encrypted snapshots are automatically encrypted.
- Volumes that you create from an unencrypted snapshot that you own or have access to can be encrypted on-the-fly.
- When you copy an unencrypted snapshot that you own, you can encrypt it during the copy process.
- When you copy an encrypted snapshot that you own or have access to, you can reencrypt it with a different key during the copy process.
- The first snapshot you take of an encrypted volume that has been created from an unencrypted snapshot is always a full snapshot.
- The first snapshot you take of a reencrypted volume, which has a different CMK compared to the source snapshot, is always a full snapshot.

#### EBS Encryption

Amazon EBS encryption is an encryption solution for your EBS volumes and snapshots. It uses AWS Key Management Service (AWS KMS) customer master keys (CMK). 

### Amazon Machine Image (AMI)

An Amazon Machine Image (AMI) is a template that contains a software configuration (for example, an operating system, an application server, and applications). From an AMI, you launch an instance, which is a copy of the AMI running as a virtual server in the cloud. You can launch multiple instances of an AMI.
- All AMIs are categorized as either backed by Amazon EBS, which means that the root device for an instance launched from the AMI is an Amazon EBS volume, or backed by instance store, which means that the root device for an instance launched from the AMI is an instance store volume created from a template stored in Amazon S3.

### EC2 Pricing Models

Amazon EC2 provides the following purchasing options to enable you to optimize your costs based on your needs:
- On-Demand Instances – Pay, by the second, for the instances that you launch. No long-term commitment required. 
- Savings Plans – Reduce your Amazon EC2 costs by making a commitment to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years.
- Reserved Instances – Reduce your Amazon EC2 costs by making a commitment to a consistent instance configuration, including instance type and Region, for a term of 1 or 3 years.
  - Standard: These provide the most significant discount, but can only be modified.
  - Convertible: These provide a lower discount than Standard Reserved Instances, but can be exchanged for another Convertible Reserved Instance with different instance attributes. Convertible Reserved Instances can also be modified.
- Scheduled Instances – Purchase instances that are always available on the specified recurring schedule, for a one-year term.
  - Scheduled Reserved Instances (Scheduled Instances) enable you to purchase capacity reservations that recur on a daily, weekly, or monthly basis, with a specified start time and duration, for a one-year term.
- Spot Instances – Request unused EC2 instances, which can reduce your Amazon EC2 costs significantly.
  - is an unused EC2 instance that is available for less than the On-Demand price
  - runs whenever capacity is available and the maximum price per hour for your request exceeds the Spot price
  - are a cost-effective choice if you can be flexible about when your applications run and if your applications can be interrupted (batch jobs, data analysis apps)
  - Spot Fleet – A set of Spot Instances that is launched based on criteria that you specify. The Spot Fleet selects the Spot Instance pools that meet your needs and launches Spot Instances to meet the target capacity for the fleet. By default, Spot Fleets are set to maintain target capacity by launching replacement instances after Spot Instances in the fleet are terminated. You can submit a Spot Fleet as a one-time request, which does not persist after the instances have been terminated. You can include On-Demand Instance requests in a Spot Fleet request.
  - recommended for stateless, fault-tolerant, flexible applications (stateless web servers, container workloads, rendering, big data)
- Dedicated Hosts – Pay for a physical host that is fully dedicated to running your instances, and bring your existing per-socket, per-core, or per-VM software licenses to reduce costs.
  - BYOL model
  - allow you to use your existing per-socket, per-core, or per-VM software licenses
  - you get to control the placement of your instances you place onto the Dedicated Host
- Dedicated Instances – Pay, by the hour, for instances that run on single-tenant hardware.
  - instances that run in a virtual private cloud (VPC) on hardware that's dedicated to a single customer
- Capacity Reservations – Reserve capacity for your EC2 instances in a specific Availability Zone for any duration.

### Instance Metadata

Instance metadata is data about your instance that you can use to configure or manage the running instance. Instance metadata is divided into categories, for example, host name, events, and security groups.

A special URL is located at `http://169.254.169.254/latest/meta-data/`

### EC2 Instance Roles

Applications must sign their API requests with AWS credentials. Therefore, if you are an application developer, you need a strategy for managing credentials for your applications that run on EC2 instances. For example, you can securely distribute your AWS credentials to the instances, enabling the applications on those instances to use your credentials to sign requests, while protecting your credentials from other users. However, it's challenging to securely distribute credentials to each instance, especially those that AWS creates on your behalf, such as Spot Instances or instances in Auto Scaling groups. You must also be able to update the credentials on each instance when you rotate your AWS credentials.

Instance Profile: Amazon EC2 uses an instance profile as a container for an IAM role. When you create an IAM role using the IAM console, the console creates an instance profile automatically and gives it the same name as the role to which it corresponds. If you use the Amazon EC2 console to launch an instance with an IAM role or to attach an IAM role to an instance, you choose the role based on a list of instance profile names.
- If you use the AWS CLI, API, or an AWS SDK to create a role, you create the role and instance profile as separate actions, with potentially different names. If you then use the AWS CLI, API, or an AWS SDK to launch an instance with an IAM role or to attach an IAM role to an instance, specify the instance profile name.
- An instance profile can contain only one IAM role. This limit cannot be increased.

---

## Containers

### Elastic Container Service (ECS) 

Amazon Elastic Container Service (Amazon ECS) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage Docker containers on a cluster. You can host your cluster on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks using the Fargate launch type. 
- is a regional service
- To deploy applications on Amazon ECS, your application components must be architected to run in containers using Docker.
- Amazon Elastic Container Registry (ECR) is a managed AWS Docker registry service. Customers can use the Docker CLI to push, pull, and manage images.

### ECS Cluster Types

An Amazon ECS cluster is a logical grouping of tasks or services. If you are running tasks or services that use the EC2 launch type, a cluster is also a grouping of container instances. If you are using capacity providers, a cluster is also a logical grouping of capacity providers. A capacity provider is used in association with a cluster to determine the infrastructure that a task runs on. When you first use Amazon ECS, a default cluster is created for you, but you can create multiple clusters in an account to keep your resources separate. A cluster manages:

- Scheduling and Orchestration
- Cluster manager
- Placement engine

Clusters are Region-specific. A cluster may contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers.

---

## Additional-EC2-Info

### AWS System Manager Parameter Store

AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, Amazon Machine Image (AMI) IDs, and license codes as parameter values. You can store values as plain text or encrypted data. You can reference Systems Manager parameters in your scripts, commands, SSM documents, and configuration and automation workflows by using the unique name that you specified when you created the parameter.
- Parameter store allows for storage of **configuration** and **secrets**
- Control and audit access at granular levels
- Improve your security posture by separating your data from your code

You can also reference parameters in a number of other AWS services, including the following:
- Amazon Elastic Compute Cloud (Amazon EC2)
- Amazon Elastic Container Service (Amazon ECS)
- AWS Secrets Manager
- AWS Lambda
- AWS CloudFormation
- AWS CodeBuild
- AWS CodePipeline
- AWS CodeDeploy

### EC2 Logging

You can monitor your instances using Amazon CloudWatch, which collects and processes raw data from Amazon EC2 into readable, near real-time metrics. These statistics are recorded for a period of 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing. 

By default, Amazon EC2 sends metric data to CloudWatch in 5-minute periods. To send metric data for your instance to CloudWatch in 1-minute periods, you can enable detailed monitoring on the instance.

By default, your instance is enabled for basic monitoring. You can optionally enable detailed monitoring. After you enable detailed monitoring, the Amazon EC2 console displays monitoring graphs with a 1-minute period for the instance.

Basic monitoring
- Data is available automatically in 5-minute periods at no charge.

Detailed monitoring
- Data is available in 1-minute periods for an additional charge.
- To get this level of data, you must specifically enable it for the instance. For the instances where you've enabled detailed monitoring, you can also get aggregated data across groups of similar instances.

### EC2 Placement Groups

#### Cluster Placement

When you launch a new EC2 instance, the EC2 service attempts to place the instance in such a way that all of your instances are spread out across underlying hardware to minimize correlated failures. You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload. Depending on the type of workload, you can create a placement group using one of the following placement strategies:

1. Cluster – packs instances close together inside an Availability Zone. This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typical of HPC applications.
- Cluster placement groups are recommended for applications that benefit from low network latency, high network throughput, or both. They are also recommended when the majority of the network traffic is between the instances in the group.

2. Partition – spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka.
- Partition placement groups can be used to deploy large distributed and replicated workloads, such as HDFS, HBase, and Cassandra, across distinct racks.

3. Spread – strictly places a small group of instances across distinct underlying hardware to reduce correlated failures.
- Spread placement groups are recommended for applications that have a small number of critical instances that should be kept separate from each other. 

### Enhanced Networking

Enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces. Enhanced networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies. There is no additional charge for using enhanced networking.

---

## Route53

### Overview

Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service. You can use Route 53 to perform three main functions in any combination: domain registration, DNS routing, and health checking. Do these in order if you do all 3:
1. Register domain names
2. Route internet traffic to the resources for your domain
3. Check the health of your resources

### Route 53 Health Checks

An overview of the concepts that are related to Amazon Route 53 health checking:
- DNS failover
  - A method for routing traffic away from unhealthy resources and to healthy resources. When you have more than one resource performing the same function—for example, more than one web server or mail server—you can configure Route 53 health checks to check the health of your resources and configure records in your hosted zone to route traffic only to healthy resources.

- Endpoint
  - The resource, such as a web server or an email server, that you configure a health check to monitor the health of. You can specify an endpoint by IPv4 address (192.0.2.243), by IPv6 address (2001:0db8:85a3:0000:0000:abcd:0001:2345), or by domain name (example.com).

- Health Check
  - Monitor whether a specified endpoint, such as a web server, is healthy
  - Optionally, get notified when an endpoint becomes unhealthy
  - Optionally, configure DNS failover, which allows you to reroute internet traffic from an unhealthy resource to a healthy resource

You can create three types of Amazon Route 53 health checks:

1. Health checks that monitor an endpoint
- You can configure a health check that monitors an endpoint that you specify either by IP address or by domain name. At regular intervals that you specify, Route 53 submits automated requests over the internet to your application, server, or other resource to verify that it's reachable, available, and functional. Optionally, you can configure the health check to make requests similar to those that your users make, such as requesting a web page from a specific URL.

2. Health checks that monitor other health checks (calculated health checks)
- You can create a health check that monitors whether Route 53 considers other health checks healthy or unhealthy. One situation where this might be useful is when you have multiple resources that perform the same function, such as multiple web servers, and your chief concern is whether some minimum number of your resources are healthy. You can create a health check for each resource without configuring notification for those health checks. Then you can create a health check that monitors the status of the other health checks and that notifies you only when the number of available web resources drops below a specified threshold.

3. Health checks that monitor CloudWatch alarms
- You can create CloudWatch alarms that monitor the status of CloudWatch metrics, such as the number of throttled read events for an Amazon DynamoDB database or the number of Elastic Load Balancing hosts that are considered healthy. After you create an alarm, you can create a health check that monitors the same data stream that CloudWatch monitors for the alarm.

### Route 53 Routing Policies 

- Simple routing policy – Use for a single resource that performs a given function for your domain, for example, a web server that serves content for the example.com website.
  - Simple routing lets you configure standard DNS records, with no special Route 53 routing such as weighted or latency. With simple routing, you typically route traffic to a single resource, for example, to a web server for your website.
- Failover routing policy – Use when you want to configure active-passive failover.
  - Failover routing lets you route traffic to a resource when the resource is healthy or to a different resource when the first resource is unhealthy. The primary and secondary records can route traffic to anything from an Amazon S3 bucket that is configured as a website to a complex tree of records.
- Geolocation routing policy – Use when you want to route traffic based on the location of your users.
  - Geolocation routing lets you choose the resources that serve your traffic based on the geographic location of your users
  - You can also use geolocation routing to restrict distribution of content to only the locations in which you have distribution rights. 
  - can specify geographic locations by continent, by country, or by state in the United States
  - Geolocation works by mapping IP addresses to locations
- Geoproximity routing policy (traffic flow only) – Use when you want to route traffic based on the location of your resources and, optionally, shift traffic from resources in one location to resources in another.
  - lets Amazon Route 53 route traffic to your resources based on the geographic location of your users and your resources
  - To use geoproximity routing, you must use Route 53 traffic flow
  - You can also optionally choose to route more traffic or less to a given resource by specifying a value, known as a bias. A bias expands or shrinks the size of the geographic region from which traffic is routed to a resource.
- Latency routing policy – Use when you have resources in multiple AWS Regions and you want to route traffic to the region that provides the best latency.
  - To use latency-based routing, you create latency records for your resources in multiple AWS Regions
  - Latency-based routing is based on latency measurements performed over a period of time
- Multivalue answer routing policy – Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.
  - lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries
  - multivalue answer routing also lets you check the health of each resource, so Route 53 returns only values for healthy resources
- Weighted routing policy – Use to route traffic to multiple resources in proportions that you specify.
  - lets you associate multiple resources with a single domain name (example.com) or subdomain name (acme.example.com) and choose how much traffic is routed to each resource
  - use for load balancing and testing new software versions (A/B testing, green/blue deployment)

---

## Amazon-RDS

### Relational Database Service (RDS)

Amazon Relational Database Service (Amazon RDS) is a web service that makes it easier to set up, operate, and scale a relational database in the AWS Cloud. It provides cost-efficient, resizable capacity for an industry-standard relational database and manages common database administration tasks.
- manages backups, software patching, automatic failure detection, and recovery
- high availability with a primary instance and a synchronous secondary instance that you can fail over to when problems occur
- can also use MySQL, MariaDB, or PostgreSQL read replicas to increase read scaling (requests to primary DB instances)
- already use database products you're familiar with: MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL Server

#### DB Instances

The basic building block of Amazon RDS is the DB instance. A DB instance is an isolated database environment in the AWS Cloud. Your DB instance can contain multiple user-created databases. You can access your DB instance by using the same tools and applications that you use with a standalone database instance. You can create and modify a DB instance by using the AWS Command Line Interface, the Amazon RDS API, or the AWS Management Console.

Each DB instance runs a DB engine. Amazon RDS currently supports the MySQL, MariaDB, PostgreSQL, Oracle, and Microsoft SQL Server DB engines. Each DB engine has its own supported features, and each version of a DB engine may include specific features. Additionally, each DB engine has a set of parameters in a DB parameter group that control the behavior of the databases that it manages.

### RDS Multi-AZ (HA and DR)

You can run your DB instance in several Availability Zones, an option called a Multi-AZ deployment. When you choose this option, Amazon automatically provisions and maintains a secondary standby DB instance in a different Availability Zone. Your primary DB instance is synchronously replicated across Availability Zones to the secondary instance. This approach helps provide data redundancy and failover support, eliminate I/O freezes, and minimize latency spikes during system backups.
- Multi-AZ deployments for MariaDB, MySQL, Oracle, and PostgreSQL DB instances use Amazon's failover technology. SQL Server DB instances use SQL Server Database Mirroring (DBM) or Always On Availability Groups (AGs).
- recommended to use Provisioned IOPS and DB instance classes that are optimized for Provisioned IOPS for fast, consistent performance

### RDS Backup and Restore

RDS Backups
Amazon RDS creates and saves automated backups of your DB instance during the backup window of your DB instance. RDS creates a storage volume snapshot of your DB instance, backing up the entire DB instance and not just individual databases. RDS saves the automated backups of your DB instance according to the backup retention period that you specify. If necessary, you can recover your database to any point in time during the backup retention period.
- backups are stored in S3

RDS Restore
Amazon RDS creates a storage volume snapshot of your DB instance, backing up the entire DB instance and not just individual databases. You can create a DB instance by restoring from this DB snapshot. When you restore the DB instance, you provide the name of the DB snapshot to restore from, and then provide a name for the new DB instance that is created from the restore. You can't restore from a DB snapshot to an existing DB instance; a new DB instance is created when you restore.
- You can't restore a DB instance from a DB snapshot that is both shared and encrypted. Instead, you can make a copy of the DB snapshot and restore the DB instance from the copy.

### RDS Read Replicas

Amazon RDS Read Replicas provide enhanced performance and durability for RDS database (DB) instances. They make it easy to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. You can create one or more replicas of a given source DB Instance and serve high-volume application read traffic from multiple copies of your data, thereby increasing aggregate read throughput. Read replicas can also be promoted when needed to become standalone DB instances. Read replicas are available in Amazon RDS for MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server as well as Amazon Aurora.
- Main purpose is scalability
- Asynchronous replication
- All read replicas are accessible and can be used for readscaling
- Can be within an Availability Zone, Cross-AZ, or Cross-Region

### Amazon Aurora

Amazon Aurora (Aurora) is a fully managed relational database engine that's compatible with MySQL and PostgreSQL. You already know how MySQL and PostgreSQL combine the speed and reliability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. The code, tools, and applications you use today with your existing MySQL and PostgreSQL databases can be used with Aurora. With some workloads, Aurora can deliver up to five times the throughput of MySQL and up to three times the throughput of PostgreSQL without requiring changes to most of your existing applications.
- includes a high-performance storage subsystem
- storage grows automatically as needed, up to 64TB
- automates and standardizes database clustering and replication
- Aurora is a good choice for your important enterprise data when your main objectives are reliability and high availability

#### Aurora Endpoints

An endpoint is represented as an Aurora-specific URL that contains a host address and a port. The following types of endpoints are available from an Aurora DB cluster.

Cluster endpoint
- A cluster endpoint (or writer endpoint) for an Aurora DB cluster connects to the current primary DB instance for that DB cluster. This endpoint is the only one that can perform write operations such as DDL statements. Because of this, the cluster endpoint is the one that you connect to when you first set up a cluster or when your cluster only contains a single DB instance.

Reader endpoint
- A reader endpoint for an Aurora DB cluster provides load-balancing support for read-only connections to the DB cluster. Use the reader endpoint for read operations, such as queries. By processing those statements on the read-only Aurora Replicas, this endpoint reduces the overhead on the primary instance. It also helps the cluster to scale the capacity to handle simultaneous SELECT queries, proportional to the number of Aurora Replicas in the cluster. Each Aurora DB cluster has one reader endpoint.

Custom endpoint
- A custom endpoint for an Aurora cluster represents a set of DB instances that you choose. When you connect to the endpoint, Aurora performs load balancing and chooses one of the instances in the group to handle the connection. You define which instances this endpoint refers to, and you decide what purpose the endpoint serves.

Instance endpoint
- An instance endpoint connects to a specific DB instance within an Aurora cluster. Each DB instance in a DB cluster has its own unique instance endpoint. So there is one instance endpoint for the current primary DB instance of the DB cluster, and there is one instance endpoint for each of the Aurora Replicas in the DB cluster.

### Aurora Global Database

For high availability across multiple AWS Regions, you can set up Aurora global databases. Each Aurora global database spans multiple AWS Regions, enabling low latency global reads and disaster recovery from outages across an AWS Region. Aurora automatically handles replicating all data and updates from the primary AWS Region to each of the secondary Regions.

An Aurora global database consists of one primary AWS Region where your data is mastered, and up to five read-only, secondary AWS Regions. Aurora replicates data to the secondary AWS Regions with typical latency of under a second. You issue write operations directly to the primary DB instance in the primary AWS Region.

An Aurora global database uses dedicated infrastructure to replicate your data, leaving database resources available entirely to serve application workloads. Applications with a worldwide footprint can use reader instances in the secondary AWS Regions for low-latency reads. In some unlikely cases, your database might become degraded or isolated in an AWS Region. If this happens, with a global database you can promote one of the secondary AWS Regions to take full read/write workloads in under a minute.

The Aurora cluster in the primary AWS Region where your data is mastered performs both read and write operations. The clusters in the secondary Regions enable low-latency reads. You can scale up the secondary clusters independently by adding one of more DB instances (Aurora Replicas) to serve read-only workloads. For disaster recovery, you can remove a secondary cluster from the Aurora global database and promote it to allow full read and write operations.

### Database Migration Service (DMS)

AWS Database Migration Service helps you migrate databases to AWS quickly and securely. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database. The AWS Database Migration Service can migrate your data to and from most widely used commercial and open-source databases.
- supports homogeneous migrations such as Oracle to Oracle, as well as heterogeneous migrations between different database platforms, such as Oracle or Microsoft SQL Server to Amazon Aurora
- Migrations can be from on-premises databases to Amazon RDS or Amazon EC2, databases running on EC2 to RDS
- highly resilient and self–healing
- All data changes to the source database that occur during the migration are continuously replicated to the target

---

## Network-Storage-EFS

### Elastic File System (EFS)

Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic NFS file system for use with AWS Cloud services and on-premises resources. Amazon EFS supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol. Amazon EFS o?ers two storage classes, Standard and Infrequent Access. The Standard storage class is used to store frequently accessed files. The Infrequent Access (IA) storage class is a lower-cost storage class that's designed for storing long-lived, infrequently accessed ?les cost-e?ectively.
- control access to your file systems through Portable Operating System Interface (POSIX) permissions
- can access your Amazon EFS file system concurrently from multiple NFS clients
- primarily used with EC2 Linux instances
- work with the files and directories just as you do with a local file system

---

## HA-Scale

### Elastic Load Balancing (ELB) 

Elastic Load Balancing distributes incoming application or network traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, in multiple Availability Zones. Elastic Load Balancing scales your load balancer as traffic to your application changes over time. It can automatically scale to the vast majority of workloads.

A load balancer serves as the single point of contact for clients. The load balancer distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones. This increases the availability of your application. You add one or more listeners to your load balancer.

A listener checks for connection requests from clients, using the protocol and port that you configure. The rules that you define for a listener determine how the load balancer routes requests to its registered targets. Each rule consists of a priority, one or more actions, and one or more conditions. When the conditions for a rule are met, then its actions are performed. You must define a default rule for each listener, and you can optionally define additional rules.

Each target group routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number that you specify. You can register a target with multiple target groups. You can configure health checks on a per target group basis. Health checks are performed on all targets registered to a target group that is specified in a listener rule for your load balancer.

Elastic Load Balancing supports three types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers.

### Application Load Balancer (ALB)

An Application Load Balancer functions at the application layer, the seventh layer of the Open Systems Interconnection (OSI) model. After the load balancer receives a request, it evaluates the listener rules in priority order to determine which rule to apply, and then selects a target from the target group for the rule action. 

Use cases:
- Support for path-based routing. You can configure rules for your listener that forward requests based on the URL in the request.
- Support for host-based routing. You can configure rules for your listener that forward requests based on the host field in the HTTP header. 
- Support for routing requests to multiple applications on a single EC2 instance
- Support for registering Lambda functions as targets
- Support for containerized applications - Amazon ECS

### Launch Configuration and Templates

A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. If you've launched an EC2 instance before, you specified the same information in order to launch the instance.
- you can specify your launch configuration with multiple Auto Scaling groups

### Autoscaling Groups

mazon EC2 Auto Scaling helps you ensure that you have the correct number of Amazon EC2 instances available to handle the load for your application. You create collections of EC2 instances, called Auto Scaling groups. You can specify the minimum number of instances in each Auto Scaling group, and Amazon EC2 Auto Scaling ensures that your group never goes below this size. 

Key components:
- Groups - EC2 instances are organized into groups so that they can be treated as a logical unit for the purposes of scaling and management
- Configuration Templates - can specify information such as the AMI ID, instance type, key pair, security groups, and block device mapping for your instances
- Scaling Options - several ways to scale instances
  - Maintain current instance levels at all times
  - Scale manually
  - Scale based on a schedule
  - Scale based on demand
  - Predictive scaling

#### Scaling Policies

A scaling policy instructs Amazon EC2 Auto Scaling to track a specific CloudWatch metric, and it defines what action to take when the associated CloudWatch alarm is in ALARM. When a scaling policy is executed, if the capacity calculation produces a number outside of the minimum and maximum size range of the group, Amazon EC2 Auto Scaling ensures that the new capacity never goes outside of the minimum and maximum size limits. Capacity is measured in one of two ways: using the same units that you chose when you set the desired capacity in terms of instances, or using capacity units. 

- Target tracking scaling — Increase or decrease the current capacity of the group based on a target value for a specific metric. This is similar to the way that your thermostat maintains the temperature of your home—you select a temperature and the thermostat does the rest.
- Step scaling — Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach.
- Simple scaling — Increase or decrease the current capacity of the group based on a single scaling adjustment.

### Network Load Balancer (NLB)

A Network Load Balancer functions at the fourth layer of the Open Systems Interconnection (OSI) model. It can handle millions of requests per second. 

For TCP traffic, the load balancer selects a target using a flow hash algorithm based on the protocol, source IP address, source port, destination IP address, destination port, and TCP sequence number. The TCP connections from a client have different source ports and sequence numbers, and can be routed to different targets. Each individual TCP connection is routed to a single target for the life of the connection.

For UDP traffic, the load balancer selects a target using a flow hash algorithm based on the protocol, source IP address, source port, destination IP address, and destination port. A UDP flow has the same source and destination, so it is consistently routed to a single target throughout its lifetime. Different UDP flows have different source IP addresses and ports, so they can be routed to different targets.

### Cross-zone Load Balancing

With cross-zone load balancing, each load balancer node for your Classic Load Balancer distributes requests evenly across the registered instances in all enabled Availability Zones. Cross-zone load balancing reduces the need to maintain equivalent numbers of instances in each enabled Availability Zone, and improves your application's ability to handle the loss of one or more instances. However, we still recommend that you maintain approximately equivalent numbers of instances in each enabled Availability Zone for higher fault tolerance. 

#### Sticky Sessions

By default, a Classic Load Balancer routes each request independently to the registered instance with the smallest load. However, you can use the sticky session feature (also known as session affinity), which enables the load balancer to bind a user's session to a specific instance. This ensures that all requests from the user during the session are sent to the same instance.

Requirements
- An HTTP/HTTPS load balancer
- At least 1 healthy instance in each Availability Zone

The load balancer uses a special cookie, AWSELB, to track the instance for each request to each listener. When the load balancer receives a request, it first checks to see if this cookie is present in the request. If so, the request is sent to the instance specified in the cookie. If there is no cookie, the load balancer chooses an instance based on the existing load balancing algorithm. If an instance fails or becomes unhealthy, the load balancer stops routing requests to that instance, and chooses a new healthy instance based on the existing load balancing algorithm. The request is routed to the new instance as if there is no cookie and the session is no longer sticky.

---

## Serverless-Services 

### AWS Lambda

Lambda is a compute service that lets you run code without provisioning or managing servers. Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second. You can use AWS Lambda to run your code in response to events, such as changes to data in an Amazon S3 bucket or an Amazon DynamoDB table; to run your code in response to HTTP requests using Amazon API Gateway; or invoke your code using API calls made using AWS SDKs.
- charged for every 100ms your code executes and the number of times your code is triggered

Use Cases:
- data processing
- real-time file processing
- real-time stream processing
- machine learning
- IoT and mobile backend applications

### CloudWatch 

Amazon CloudWatch monitors your Amazon Web Services (AWS) resources and the applications you run on AWS in real time. You can use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications.
- basically a metrics repository

#### CloudWatch Key Concepts

**Namespaces**
A namespace is a container for CloudWatch metrics. Metrics in different namespaces are isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics.

**Metrics**
Metrics are the fundamental concept in CloudWatch. A metric represents a time-ordered set of data points that are published to CloudWatch.

**Dimensions**
A dimension is a name/value pair that is part of the identity of a metric. You can assign up to 10 dimensions to a metric.

**Statistics**
Statistics are metric data aggregations over specified periods of time. CloudWatch provides statistics based on the metric data points provided by your custom data or provided by other AWS services to CloudWatch.

**Percentiles**
A percentile indicates the relative standing of a value in a dataset. For example, the 95th percentile means that 95 percent of the data is lower than this value and 5 percent of the data is higher than this value. Percentiles help you get a better understanding of the distribution of your metric data.

**Alarms**
You can use an alarm to automatically initiate actions on your behalf. An alarm watches a single metric over a specified time period, and performs one or more specified actions, based on the value of the metric relative to a threshold over time. The action is a notification sent to an Amazon SNS topic or an Auto Scaling policy. You can also add alarms to dashboards.

### Application Programming Interface (API) Gateway

Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale. API Gateway acts as a "front door" for applications to access data, business logic, or functionality from your backend services API Gateway creates RESTful APIs that:
- Are HTTP-based.
- Enable stateless client-server communication.
- Implement standard HTTP methods such as GET, POST, PUT, PATCH, and DELETE.

### API Gateway Use Cases

- Use API Gateway to create REST APIs - made up of resources and methods
- Use API Gateway to create WebSocket APIs - great for chat applications, real-time dashboards, real-time alerts and notifications
- Use API Gateway to create HTTP APIs: use to send API requests to Lambda or to a public HTTP endpoint

### Simple Notification Service (SNS)

Amazon Simple Notification Service (Amazon SNS) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients. In Amazon SNS, there are two types of clients—publishers and subscribers—also referred to as producers and consumers.

![SNS Overview](https://docs.aws.amazon.com/sns/latest/dg/images/sns-how-works.png "SNS Arch")

Common scenarios:
1. Fanout - The "fanout" scenario is when an Amazon SNS message is sent to a topic and then replicated and pushed to multiple Amazon SQS queues, HTTP endpoints, or email addresses. 
2. Application and system alerts - Application and system alerts are notifications that are triggered by predefined thresholds and sent to specified users by SMS and/or email.
3. Push email and text messaging - Push email and text messaging are two ways to transmit messages to individuals or groups via email and/or SMS. 
4. Mobile push notifications - Mobile push notifications enable you to send messages directly to mobile apps.
5. Message durability - Amazon SNS provides durable storage of all messages that it receives. When Amazon SNS receives your Publish request, it stores multiple copies of your message to disk.

### Simple Queue Service (SQS)

Amazon Simple Queue Service (Amazon SQS) offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components. Amazon SQS offers common constructs such as dead-letter queues and cost allocation tags. It provides a generic web services API and it can be accessed by any programming language that the AWS SDK supports. Amazon SQS supports both standard and FIFO queues. 

Standard Queues
- Unlimited throughput
- At-least-once Delivery
- Best-effort Ordering

FIFO Queues
- High throughput (3,000 transactions per second)
- Exactly-once Processing
- First-in-First-out Delivery (FIFO)

### Kinesis

Amazon Kinesis makes it easy to collect, process, and analyze video and data streams in real time.

You can use Amazon Kinesis Data Streams to collect and process large streams of data records in real time. You can create data-processing applications, known as Kinesis Data Streams applications. A typical Kinesis Data Streams application reads data from a data stream as data records. 
- Accelerated log and data feed intake and processing
- Real-time metrics and reporting
- Real-time data analytics
- Complex stream processing

Amazon Kinesis Video Streams is a fully managed AWS service that you can use to stream live video from devices to the AWS Cloud, or build applications for real-time video processing or batch-oriented video analytics. Kinesis Video Streams isn't just storage for video data. You can use it to watch your video streams in real time as they are received in the cloud. 
- Stream from millions of devices, plus stream securely
- index data (media)
- Build real-time and batch applications

### SQS compared to Kinesis

Kinesis

- scales well
- can handle large amounts of data (think volume)
- connects multiple consumers to a stream concurrently
- maintains order of record

SQS

- very reliable
- serverless architecture
- batch sending of messages
- message delay up to 15 mins

---

## CDN-Network-Optimization

### CloudFront

Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.

Use cases:
- Accelerate static website content delivery: Using S3 together with CloudFront has a number of advantages, including the option to use Origin Access Identity (OAI) to easily restrict access to your S3 content.
- Serve video on demand or live streaming video: For broadcasting a live stream, you can cache media fragments at the edge, so that multiple requests for the manifest file that delivers the fragments in the right order can be combined, to reduce the load on your origin server.
- Encrypt specific fields throughout system processing: secure end-to-end connections to origin servers
- Customize at the edge: For example, you can return a custom error message when your origin server is down for maintenance, so viewers don't get a generic HTTP error message. 
- Serve private content by using Lambda@Edge customizations: configure your CloudFront distribution to serve private content from your own custom origin

### Origin Access Identity (OAI)

When you first set up an Amazon S3 bucket as the origin for a CloudFront distribution, you grant everyone permission to read the files in your bucket. This allows anyone to access your files either through CloudFront or using the Amazon S3 URL. CloudFront doesn't expose Amazon S3 URLs, but your users might have those URLs if your application serves any files directly from Amazon S3 or if anyone gives out direct links to specific files in Amazon S3.

To restrict access to content that you serve from Amazon S3 buckets, follow these steps:
1. Create a special CloudFront user called an origin access identity (OAI) and associate it with your distribution.
2. Configure your S3 bucket permissions so that CloudFront can use the OAI to access the files in your bucket and serve them to your users. Make sure that users can’t use a direct URL to the S3 bucket to access a file there.

### AWS Global Accelerator

AWS Global Accelerator is a service in which you create accelerators to improve availability and performance of your applications for local and global users. Global Accelerator directs traffic to optimal endpoints over the AWS global network. Global Accelerator is a global service that supports endpoints in multiple AWS Regions. Global Accelerator uses the AWS global network to route traffic to the optimal regional endpoint based on health, client location, and policies that you configure. The service reacts instantly to changes in health or configuration to ensure that internet traffic from clients is always directed to healthy endpoints.

#### Main Components

**Accelerator**
An accelerator directs traffic to optimal endpoints over the AWS global network to improve the availability and performance of your internet applications. Each accelerator includes one or more listeners.

**Listener**
A listener processes inbound connections from clients to Global Accelerator, based on the port (or port range) and protocol that you configure.

**Endpoint**
Endpoints can be Network Load Balancers, Application Load Balancers, EC2 instances, or Elastic IP addresses. An Application Load Balancer endpoint can be an internet-facing or internal. Traffic is routed to endpoints based on configuration options that you choose, such as endpoint weights. For each endpoint, you can configure weights, which are numbers that you can use to specify the proportion of traffic to route to each one. This can be useful, for example, to do performance testing within a Region.

---

## Other-VPC-Topics

### VPC Flow Logs

VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data can be published to Amazon CloudWatch Logs or Amazon S3. After you've created a flow log, you can retrieve and view its data in the chosen destination. Flow logs can help with the following:

- Diagnosing overly restrictive security group rules
- Monitoring the traffic that is reaching your instance
- Determining the direction of the traffic to and from the network interfaces

You can create a flow log for a VPC, a subnet, or a network interface. If you create a flow log for a subnet or VPC, each network interface in that subnet or VPC is monitored. By default, the log line format for a flow log record is a space-separated string that has the following set of fields in the following order.

`<version> <account-id> <interface-id> <srcaddr> <dstaddr> <srcport> <dstport> <protocol> <packets> <bytes> <start> <end> <action> <log-status>`


### IPv6 with Egress-only IGW

An egress-only internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows outbound communication over IPv6 from instances in your VPC to the internet, and prevents the internet from initiating an IPv6 connection with your instances. Note, an egress-only internet gateway is for use with IPv6 traffic only. To enable outbound-only internet communication over IPv4, use a NAT gateway instead. 

An egress-only internet gateway has the following characteristics:
- You cannot associate a security group with an egress-only internet gateway. You can use security groups for your instances in the private subnet to control the traffic to and from those instances.
- You can use a network ACL to control the traffic to and from the subnet for which the egress-only internet gateway routes traffic.


### VPC Endpoints

A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

Endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components. They allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic.

Key concepts:
- Endpoint service — Your own application in your VPC. Other AWS principals can create a connection from their VPC to your endpoint service
- Gateway endpoint — A gateway endpoint is a gateway that you specify as a target for a route in your route table for traffic destined to a supported AWS service.
  - S3 
  - DynamoDB
- Interface endpoint — An interface endpoint is an elastic network interface (ENI) with a private IP address from the IP address range of your subnet that serves as an entry point for traffic destined to a supported service.
  - powered by AWS PrivateLink (restricts all network traffic between your VPC and services to the Amazon network)
  - LOTS of [AWS services](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) are supported

### VPC Peering

A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them privately. Instances in either VPC can communicate with each other as if they are within the same network. You can create a VPC peering connection between your own VPCs, with a VPC in another AWS account, or with a VPC in a different AWS Region.

AWS uses the existing infrastructure of a VPC to create a VPC peering connection; it is neither a gateway nor an AWS Site-to-Site VPN connection, and does not rely on a separate piece of physical hardware. There is no single point of failure for communication or a bandwidth bottleneck. A VPC peering connection helps you to facilitate the transfer of data.
- You can establish peering relationships between VPCs across different AWS Regions (also called Inter-Region VPC Peering)
- traffic remains in the private IP space
- Inter-Region VPC Peering provides a simple and cost-effective way to share resources between regions or replicate data for geographic redundancy

---

## Migration

### AWS Site-to-Site VPN

Although the term VPN connection is a general term, in this documentation, a VPN connection refers to the connection between your VPC and your own on-premises network. Site-to-Site VPN supports Internet Protocol security (IPsec) VPN connections. A Site-to-Site VPN connection offers two VPN tunnels between a virtual private gateway or a transit gateway on the AWS side, and a customer gateway (which represents a VPN device) on the remote (on-premises) side.

A Site-to-Site VPN connection has the following limitations:
- IPv6 traffic is not supported.
- An AWS VPN connection does not support Path MTU Discovery.

Concepts
- Virtual private gateway: A virtual private gateway is the VPN concentrator on the Amazon side of the Site-to-Site VPN connection. You create a virtual private gateway and attach it to the VPC from which you want to create the Site-to-Site VPN connection.
- Transit gateway: A transit gateway is a transit hub that you can use to interconnect your virtual private clouds (VPC) and on-premises networks. For more information, see Amazon VPC Transit Gateways. You can create a Site-to-Site VPN connection as an attachment on a transit gateway.
- Customer gateway: A customer gateway is a resource that you create in AWS that represents the customer gateway device in your on-premises network.

### AWS Direct Connect (DX)

AWS Direct Connect links your internal network to an AWS Direct Connect location over a standard Ethernet fiber-optic cable. One end of the cable is connected to your router, the other to an AWS Direct Connect router. With this connection, you can create virtual interfaces directly to public AWS services (for example, to Amazon S3) or to Amazon VPC, bypassing internet service providers in your network path. An AWS Direct Connect location provides access to AWS in the Region with which it is associated. You can use a single connection in a public Region or AWS GovCloud (US) to access public AWS services in all other public Regions.
- supports both the IPv4 and IPv6

There are 2 types of connections:
1. Dedicated Connection: A physical Ethernet connection associated with a single customer. Customers can request a dedicated connection through the AWS Direct Connect console, the CLI, or the API.
- 1Gbps or 10Gbps connection
2. Hosted Connection: A physical Ethernet connection that an AWS Direct Connect Partner provisions on behalf of a customer. Customers request a hosted connection by contacting a partner in the AWS Direct Connect Partner Program, who provisions the connection.
- 50Mbps, 100Mbps, 200Mbps, 300Mbps, 400Mbps, 500Mbps, 1Gbps, 2Gbps, 5Gbps, and 10Gbps options

### Storage Gateway

AWS Storage Gateway connects an on-premises software appliance with cloud-based storage to provide seamless integration with data security features between your on-premises IT environment and the AWS storage infrastructure. You can use the service to store data in the AWS Cloud for scalable and cost-effective storage that helps maintain data security.

**File Gateway** – A file gateway supports a file interface into Amazon Simple Storage Service (Amazon S3) and combines a service and a virtual software appliance.
- NFS and SMB
- can access your data directly in Amazon S3, including using lifecycle policies, cross-region replication, and versioning

**Volume Gateway** – A volume gateway provides cloud-backed storage volumes that you can mount as Internet Small Computer System Interface (iSCSI) devices from your on-premises application servers.
- deployed into your on-premises environment as a VM running on VMware ESXi, KVM, or Microsoft Hyper-V
  - Cached volumes – You store your data in Amazon Simple Storage Service (Amazon S3) and retain a copy of frequently accessed data subsets locally.
  - Stored volumes – If you need low-latency access to your entire dataset, first configure your on-premises gateway to store all your data locally.

**Tape Gateway** – A tape gateway provides cloud-backed virtual tape storage. The tape gateway is deployed into your on-premises environment as a VM running on VMware ESXi, KVM, or Microsoft Hyper-V hypervisor.
- archive backup data in GLACIER or DEEP_ARCHIVE
- can run on-premises as a VM appliance, as a hardware appliance, or in AWS as an Amazon EC2 instance

### Snowball Options

#### Snowball

The AWS Snowball service uses physical storage devices to transfer large amounts of data between Amazon Simple Storage Service (Amazon S3) and your onsite data storage location at faster-than-internet speeds.
- use AWS Key Management Service (AWS KMS)
- Snowball is intended for transferring large amounts of data. If you want to transfer less than 10 TB of data between your on-premises data centers and Amazon S3, Snowball might not be your most economical choice.
- 80 TB and 50 TB models
- uses enforced encryption for data at-rest and data in-transit
- uses the Snowball client to transfer data to/from device

#### Snowball Edge

The AWS Snowball Edge is a type of Snowball device with on-board storage and compute power for select AWS capabilities. Snowball Edge can undertake local processing and edge-computing workloads in addition to transferring data between your local environment and the AWS Cloud. Snowball Edge devices have three options for device configurations – storage optimized, compute optimized, and with GPU. 
- transfer speeds of up to 100 GB/second
- can import or export data between your local environments and Amazon S3, physically transporting the data with one or more devices, completely bypassing the internet
- read/write data using NFS mount point
- have Amazon S3 and Amazon EC2 compatible endpoints available

Device options:
- Snowball Edge Storage Optimized (for data transfer) – This Snowball Edge device option has a 100 TB (80 TB usable) storage capacity.
- Snowball Edge Storage Optimized (with EC2 compute functionality) – This Snowball Edge device option has up to 80 TB of usable storage space, 24 vCPUs, and 32 GiB of memory for compute functionality. It also comes with 1 TB of additional SSD storage space for block volumes attached to Amazon EC2 AMIs.
- Snowball Edge Compute Optimized – This Snowball Edge device option has the most compute functionality, with 52 vCPUs, 208 GiB of memory, and 42 TB (39.5 usable) plus 7.68 TB of dedicated NVMe SSD for compute instances for block storage volumes for EC2 compute instances, and 42 TB of HDD capacity for either object storage or block storage volumes.
- Snowball Edge Compute Optimized with GPU – This Snowball Edge device option is identical to the compute optimized option, save for an installed GPU, equivalent to the one available in the P3 Amazon EC2 instance type. It has a storage capacity of 42 TB (39.5 TB of HDD storage that can be used for a combination of Amazon S3 compatible object storage and Amazon EBS compatible block storage volumes) plus 7.68 TB of dedicated NVMe SSD for compute instances.

#### Snowmobile

AWS Snowmobile is an Exabyte-scale data transfer service used to move extremely large amounts of data to AWS. You can transfer up to 100PB per Snowmobile, a 45-foot long ruggedized shipping container, pulled by a semi-trailer truck. (WOW!)
- seriously, contact AWS to initiate this process

### AWS Directory Service

AWS Directory Service provides multiple ways to use Microsoft Active Directory (AD) with other AWS services. 
- Microsoft AD or Lightweight Directory Access Protocol (LDAP)–aware applications
- use if you need an actual Microsoft Active Directory in the AWS Cloud that supports Active Directory–aware workloads, or AWS applications and services such as Amazon WorkSpaces and Amazon QuickSight, or you need LDAP support for Linux applications.

#### Other Directory Service Options

- AD Connector: is a proxy service that provides an easy way to connect compatible AWS applications, such as Amazon WorkSpaces, Amazon QuickSight, and Amazon EC2 for Windows Server instances, to your existing on-premises Microsoft Active Directory. AD Connector is your best choice when you want to use your existing on-premises directory with compatible AWS services.

- Simple AD: is a Microsoft Active Directory–compatible directory from AWS Directory Service that is powered by Samba 4. Simple AD is a standalone directory in the cloud, where you create and manage user identities and manage access to applications. Use Simple AD as a standalone directory in the cloud to support Windows workloads that need basic AD features, compatible AWS applications, or to support Linux workloads that need LDAP service.

- Amazon Cognito: is a user directory that adds sign-up and sign-in to your mobile app or web application using Amazon Cognito User Pools. 

### AWS DataSync

AWS DataSync is an online data transfer service that simplifies, automates, and accelerates copying large amounts of data to and from AWS storage services over the internet or AWS Direct Connect. DataSync can copy data between Network File System (NFS), Server Message Block (SMB) file servers, or AWS Snowcone, and Amazon Simple Storage Service (Amazon S3) buckets, Amazon EFS file systems, and Amazon FSx for Windows File Server file systems.
- rates up to 10Gbps
- can transfer data between SMB file shares and Amazon S3, Amazon EFS or Amazon FSx for Windows File Server
- supports Network File System (NFS), Server Message Block (SMB), Amazon EFS, Amazon FSx for Windows File Server, and Amazon S3 as location types

Main use cases for AWS DataSync:
- Data migration – Move active datasets rapidly over the network into Amazon S3, Amazon EFS, or Amazon FSx for Windows File Server. DataSync includes automatic encryption and data integrity validation to help make sure that your data arrives securely, intact, and ready to use.
- Data movement for timely in-cloud processing – Move data into or out of AWS for processing when working with systems that generate data on-premises. This approach can speed up critical hybrid cloud workflows across many industries. These include video production in media and entertainment, seismic research in oil and gas, machine learning in life science, and big data analytics in finance.
- Data archiving – Move cold data from expensive on-premises storage systems directly to durable and secure long-term storage such as Amazon S3 Glacier or S3 Glacier Deep Archive. By doing this, you can free up on-premises storage capacity and shut down legacy storage systems.
- Data protection – Move data into all Amazon S3 storage classes, and choose the most cost-effective storage class for your needs. You can also send the data to Amazon EFS or Amazon FSx for Windows File Server for a standby file system.

### FSx for Windows File Server

Amazon FSx for Windows File Server provides fully managed, highly reliable, and scalable file storage that is accessible over the industry-standard Server Message Block (SMB). 
- single-AZ and multi-AZ deployment options
- provides automatic durable backups
- Easy file-level restores 
- Amazon FSx provides two types of storage – Hard Disk Drives (HDD) and Solid State Drives (SSD)

Use Cases
- Home directories for users
- Lift-and-shift Windows applications
- HA Microsoft SQL Server deployments
- Media workflows
- Web serving and content management
- Data analytics

---

## Security-Operations

### AWS Secrets Manager

Secrets Manager enables you to replace hardcoded credentials in your code, including passwords, with an API call to Secrets Manager to retrieve the secret programmatically. 

Features:
- Programmatically Retrieve Encrypted Secret Values at Runtime: enables you to replace stored credentials with a runtime call to the Secrets Manager Web service, so you can retrieve the credentials dynamically when you need them.
- Storing Different Types of Secrets: enables you to store text in the encrypted secret data portion of a secret like connection details of the database
- Encrypting Your Secret Data: protects data using AWS KMS
- Automatically Rotating Your Secrets: configure Secrets Manager to automatically rotate your secrets without user intervention and on a specified schedule
- Control Access to Secrets: can attach IAM permission policies to your users, groups, and roles that grant or deny access to specific secrets, and restrict management of those secrets

### AWS Shield

AWS provides AWS Shield Standard and AWS Shield Advanced for protection against DDoS attacks. AWS Shield Standard is automatically included at no extra cost beyond what you already pay for AWS WAF and your other AWS services. For added protection against DDoS attacks, AWS offers AWS Shield Advanced. AWS Shield Advanced provides expanded DDoS attack protection for your Amazon Elastic Compute Cloud instances, Elastic Load Balancing load balancers, Amazon CloudFront distributions, Amazon Route 53 hosted zones, and your AWS Global Accelerator accelerators.

**Shield Standard**
- All AWS customers benefit from the automatic protections of AWS Shield Standard, at no additional charge. AWS Shield Standard defends against most common, frequently occurring network and transport layer DDoS attacks that target your website or applications. While AWS Shield Standard helps protect all AWS customers, you get particular benefit if you are using Amazon CloudFront and Amazon Route 53. These services receive comprehensive availability protection against all known infrastructure (Layer 3 and 4) attacks.

**Shield Advanced**
- Adds protect to ELB's, EC2 instance EIP's, CloudFront distributions, Route 53 hosted zones, and AWS Global Accelerator
- access to 24x7 DDoS response team (DRT)
- supports Layer 7 attacks

#### AWS WAF

AWS WAF is a web application firewall that lets you monitor the HTTP(S) requests that are forwarded to an Amazon CloudFront distribution, an Amazon API Gateway REST API, or an Application Load Balancer. You can also use AWS WAF to protect your applications that are hosted in Amazon Elastic Container Service (Amazon ECS) containers.

You use AWS WAF to control how an Amazon CloudFront distribution, an Amazon API Gateway REST API, or an Application Load Balancer responds to web requests.
- Web ACLs – You use a web access control list (ACL) to protect a set of AWS resources. You create a web ACL and define its protection strategy by adding rules. Rules define criteria for inspecting web requests and specify how to handle requests that match the criteria. You set a default action for the web ACL that indicates whether to block or allow through those requests that pass the rules inspections.
- Rules – Each rule contains a statement that defines the inspection criteria, and an action to take if a web request meets the criteria. When a web request meets the criteria, that's a match. You can use rules to block matching requests or to allow matching requests through. You can also use rules just to count matching requests.
- Rules groups – You can use rules individually or in reusable rule groups. AWS Managed Rules and AWS Marketplace sellers provide managed rule groups for your use. You can also define your own rule groups.

A web access control list (web ACL) gives you fine-grained control over the web requests that your Amazon CloudFront distribution, Amazon API Gateway REST API, or Application Load Balancer responds to.

You can use criteria like the following to allow or block requests:
- IP address origin of the request
- Country of origin of the request
- String match or regular expression (regex) match in a part of the request
- Size of a particular part of the request
- Detection of malicious SQL code or scripting

### CloudHSM

AWS CloudHSM provides hardware security modules in the AWS Cloud. A hardware security module (HSM) is a computing device that processes cryptographic operations and provides secure storage for cryptographic keys.

When you use an HSM from AWS CloudHSM, you can perform a variety of cryptographic tasks:
- Generate, store, import, export, and manage cryptographic keys, including symmetric keys and asymmetric key pairs.
- Use symmetric and asymmetric algorithms to encrypt and decrypt data.
- Use cryptographic hash functions to compute message digests and hash-based message authentication codes (HMACs).
- Cryptographically sign data (including code signing) and verify signatures.
- Generate cryptographically secure random data.

Use Cases:
1. Offload the SSL/TLS Processing for Web Servers
2. Protect the Private Keys for an Issuing Certificate Authority (CA)
3. Enable Transparent Data Encryption (TDE) for Oracle Databases

---

## DynamoDB

### What is DynamoDB?

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. With DynamoDB, you can create database tables that can store and retrieve any amount of data and serve any level of request traffic. DynamoDB provides on-demand backup capability. DynamoDB automatically spreads the data and traffic for your tables over a sufficient number of servers to handle your throughput and storage requirements, while maintaining consistent and fast performance. All of your data is stored on solid-state disks (SSDs) and is automatically replicated across multiple Availability Zones in an AWS Region.

#### DynamoDB Consistency Model

DynamoDB supports eventually consistent and strongly consistent reads.

**Eventually Consistent Reads**
- When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

**Strongly Consistent Reads**
- When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. However, this consistency comes with some disadvantages:
- A strongly consistent read might not be available if there is a network delay or outage. In this case, DynamoDB may return a server error (HTTP 500).
- Strongly consistent reads may have higher latency than eventually consistent reads.
- Strongly consistent reads are not supported on global secondary indexes.
- Strongly consistent reads use more throughput capacity than eventually consistent reads.

#### Partitions

Amazon DynamoDB stores data in partitions. A partition is an allocation of storage for a table, backed by solid state drives (SSDs) and automatically replicated across multiple Availability Zones within an AWS Region. Partition management is handled entirely by DynamoDB—you never have to manage partitions yourself.

### In-Memory Acceleration with DynamoDB Accelerator (DAX)

Amazon DynamoDB is designed for scale and performance. In most cases, the DynamoDB response times can be measured in single-digit milliseconds. However, there are certain use cases that require response times in microseconds. For these use cases, DynamoDB Accelerator (DAX) delivers fast response times for accessing eventually consistent data.

DAX is a DynamoDB-compatible caching service that enables you to benefit from fast in-memory performance for demanding applications. DAX addresses three core scenarios:
1. As an in-memory cache, DAX reduces the response times of eventually consistent read workloads by an order of magnitude from single-digit milliseconds to microseconds.
2. DAX reduces operational and application complexity by providing a managed service that is API-compatible with DynamoDB. Therefore, it requires only minimal functional changes to use with an existing application.
3. For read-heavy or bursty workloads, DAX provides increased throughput and potential operational cost savings by reducing the need to overprovision read capacity units. This is especially beneficial for applications that require repeated reads for individual keys.

### DynamoDB Global Tables

Amazon DynamoDB global tables provide a fully managed solution for deploying a multiregion, multi-master database, without having to build and maintain your own replication solution. With global tables you can specify the AWS Regions where you want the table to be available. DynamoDB performs all of the necessary tasks to create identical tables in these Regions and propagate ongoing data changes to all of them.

DynamoDB global tables are ideal for massively scaled applications with globally dispersed users. In such an environment, users expect very fast application performance. Global tables provide automatic multi-master replication to AWS Regions worldwide. They enable you to deliver low-latency data access to your users no matter where they are located.

As of this writing, there are two versions of DynamoDB global tables available: Version 2019.11.21 (Current and recommended) and Version 2017.11.29. 

### Amazon Athena

Athena is an interactive query service that makes it easy to analyze data directly in Amazon Simple Storage Service (Amazon S3) using standard SQL. 
- analyze unstructured, semi-structured, and structured data stored in Amazon S3
- JSON, CSV, columnar data formats
- use Athena to run ad-hoc queries using SQL statements
