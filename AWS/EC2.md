Question 1

What is Amazon EC2? Explain how it works internally.

Difficulty: ⭐⭐☆☆☆

30 Second Answer

Amazon EC2 (Elastic Compute Cloud) is AWS's Infrastructure-as-a-Service (IaaS) offering that allows you to launch virtual machines on demand. When an EC2 instance is launched, AWS provisions compute resources from an Amazon Machine Image (AMI), places the instance inside a VPC, attaches storage such as EBS, applies Security Groups and IAM Roles, boots the operating system, and makes the instance available for workloads.

Detailed Answer

When you launch an EC2 instance, AWS performs several operations in sequence.

AMI
        ↓
Instance Type
        ↓
VPC
        ↓
Subnet
        ↓
Security Group
        ↓
IAM Role
        ↓
EBS Volume
        ↓
Boot OS
        ↓
Run User Data
        ↓
Application Starts

Each stage has a purpose.

AMI

Contains:

Operating System
Installed packages
Startup configuration

Think of an AMI as a golden VM image.

Instance Type

Determines:

CPU
Memory
Network
Storage throughput

Example

Instance	Best For
t3	Development
m5	General Workloads
c6	CPU Intensive
r6	Memory Intensive
Networking

Every EC2 instance launches inside a VPC.

Typically:

Internet

↓

Internet Gateway

↓

Application Load Balancer

↓

Private EC2

Production applications should rarely expose EC2 directly to the internet.

Storage

Most production workloads use EBS.

Benefits:

Persistent
Snapshots
Encryption
High performance
Security

Two major components

Security Group

↓

Network Firewall

IAM Role

↓

Identity

Never store AWS keys on EC2.

Production Scenario

A retail application runs on four EC2 instances.

Traffic suddenly increases from

500 requests/sec

to

5000 requests/sec.

CloudWatch detects CPU >75%.

Auto Scaling launches four more EC2 instances.

Application Load Balancer begins routing traffic.

Users never notice.

AI Enhancement
AI Infrastructure Advisor

Instead of manually reviewing CloudWatch dashboards,

AI continuously evaluates

CPU
Memory
Deployment history
Application logs
Previous incidents

AI might recommend:

Current Instance

m5.large

↓

Traffic Pattern

Consistent

↓

Recommendation

Move to c6.large

Estimated Savings

18%

This combines FinOps with performance optimization.

Common Mistakes

❌ EC2 is just a VM.

Better answer:

EC2 is compute integrated with networking, security, storage, IAM, monitoring, and scaling.

Interview Follow-up Questions
Difference between AMI and Snapshot?
Difference between EC2 and ECS?
Difference between EC2 and Lambda?


---------------------------------------------------------------------------------------------------------------

Question 2
Walk me through the lifecycle of an EC2 instance.

Difficulty: ⭐⭐⭐☆☆

30 Second Answer

The EC2 lifecycle begins with selecting an AMI, launching an instance, attaching networking and storage, booting the operating system, running workloads, monitoring health, scaling when required, stopping or rebooting if needed, and finally terminating the instance.

Detailed Answer

Lifecycle

Launch

↓

Pending

↓

Running

↓

Stopping

↓

Stopped

↓

Starting

↓

Running

↓

Terminated

Important

Stopped

means

Compute gone

Storage remains.

Terminated

means

Compute deleted.

Instance Store deleted.

EBS depends on DeleteOnTermination flag.

Production Scenario

Production upgrade.

Instead of terminating instances,

Operations team

Stops

↓

Creates AMI

↓

Validates

↓

Restarts

Rollback becomes much easier.

AI Enhancement
AI Lifecycle Optimizer

AI monitors:

Business hours
CPU usage
User activity

Example

Development servers unused after 8 PM.

AI recommends

Stop

↓

Save

↓

Restart

7 AM

Cloud cost reduced automatically.

Common Mistakes

People confuse

Stopped

and

Terminated.

Interviewers ask this often.

Follow-up

What happens to public IP after Stop?


---------------------------------------------------------------------------------------------------------------

Question 3
Explain the difference between AMI and EBS Snapshot.

Difficulty

⭐⭐⭐☆☆

30 Second Answer

An AMI is a complete machine image used to launch new EC2 instances, while an EBS Snapshot is a backup of a specific EBS volume.

Detailed Answer

AMI contains

OS
Configuration
Application
Metadata

Snapshot contains

Only disk data.

Example

Production Server

↓

Take Snapshot

↓

Restore Disk

AMI

Production Server

↓

Launch identical server
Production Scenario

Application deployment failed.

Instead of rebuilding

Operations launched

Previous AMI

↓

Five minutes

↓

Production restored.

AI Enhancement
AI Golden Image Compliance

AI scans AMIs.

Checks

Outdated packages
Security patches
CVEs
CIS compliance

Before production deployment.

Common Mistakes

Snapshot

≠

Machine Image

Follow-up

Can one AMI use multiple snapshots?

Answer

Yes.

---------------------------------------------------------------------------------------------------------------

Question 4
Explain the difference between Stop, Reboot and Terminate.

Difficulty

⭐⭐☆☆☆

30 Second Answer

Reboot restarts the operating system. Stop shuts down the instance while preserving EBS volumes. Terminate permanently deletes the instance and usually deletes the root EBS volume.

Detailed Answer
Reboot

OS Restart

Instance remains

IP remains

Storage remains

Stop

Compute released

EBS remains

Elastic IP remains

Public IP changes unless Elastic IP attached.

Terminate

Instance destroyed.

Usually

EBS deleted.

Cannot restart.

Production Scenario

Production maintenance.

Need kernel patch.

Action

Stop

↓

Upgrade

↓

Start

Need application restart

Reboot

Need old server removed

Terminate
AI Enhancement
AI Maintenance Planner

AI analyzes

User traffic
Maintenance windows
Business calendar

Suggests

Best reboot window.

Common Mistakes

Thinking

Reboot

changes instance.

It doesn't.

Follow-up

Why would you Stop instead of Reboot?

---------------------------------------------------------------------------------------------------------------

Question 5
Explain Security Groups. How do they work internally?

Difficulty

⭐⭐⭐☆☆

30 Second Answer

Security Groups are stateful virtual firewalls attached to EC2 instances. They control inbound and outbound traffic. If inbound traffic is allowed, the response traffic is automatically permitted.

Detailed Answer

Example

Internet

↓

Security Group

↓

EC2

Rules

Port	Source
22	VPN Only
80	Internet
443	Internet

No rule

↓

Traffic denied.

Default

Deny inbound.

Allow outbound.

Stateful

Example

Allow

HTTPS

Inbound

Response traffic

Automatically allowed.

Unlike NACL.

Production Scenario

Application inaccessible.

Developer checks

Application

Healthy.

Problem?

Security Group

Port 443 missing.

Traffic blocked.

AI Enhancement
AI Firewall Rule Analyzer

AI continuously analyzes

Security Groups

Finds

Port 22

Open

0.0.0.0/0

Recommendation

Restrict

Corporate VPN

Confidence

99%

Risk

Critical

Instead of waiting for a security audit, engineers receive proactive recommendations.

Common Mistakes

People confuse

Security Groups

and

NACLs.

Remember

Security Group

Instance level

Stateful

NACL

Subnet level

Stateless

Interview Follow-up Questions
Can one EC2 have multiple Security Groups?
Are Security Groups stateful?
What happens if no outbound rule exists?
Difference between Security Groups and NACL?
Can Security Groups reference other Security Groups?

---------------------------------------------------------------------------------------------------------------
