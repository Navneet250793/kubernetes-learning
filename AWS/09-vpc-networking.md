# AWS VPC Networking

## What is a VPC?

Amazon Virtual Private Cloud (VPC) is a logically isolated virtual network in AWS where you launch your AWS resources.

It allows you to control:

- IP Address Range (CIDR)
- Subnets
- Route Tables
- Internet Connectivity
- Security Groups
- Network ACLs

Every AWS resource such as EC2, RDS, EKS and ALB runs inside a VPC.

---

# Why Do We Need a VPC?

Without VPC

```text
Internet

↓

All Servers

↓

No Isolation
```

Problems

- No Network Isolation
- Poor Security
- Difficult Resource Management

---

With VPC

```text
Internet

↓

AWS VPC

├── Public Subnet
├── Private Subnet
├── Route Table
├── Security Groups
└── Network ACLs
```

Benefits

- Network Isolation
- Better Security
- Controlled Traffic
- High Availability

---

# VPC Architecture

```text
                        AWS Region
                            │
                            ▼
                      Amazon VPC
                            │
          ┌─────────────────┴─────────────────┐
          ▼                                   ▼
    Public Subnet                      Private Subnet
          │                                   │
          ▼                                   ▼
     ALB / Bastion                     Worker Nodes
          │                                   │
          └───────────────┬───────────────────┘
                          ▼
                      Kubernetes Pods
```

---

# Components of a VPC

## CIDR Block

Defines the IP address range for the VPC.

Example

```text
10.0.0.0/16
```

This provides 65,536 private IP addresses.

---

## Subnets

A subnet is a smaller network inside a VPC.

Example

```text
10.0.1.0/24
```

Public Subnet

- ALB
- NAT Gateway
- Bastion Host

Private Subnet

- Worker Nodes
- Databases
- Internal Applications

---

## Internet Gateway (IGW)

Provides Internet access to resources in Public Subnets.

```text
Internet

↓

Internet Gateway

↓

Public Subnet
```

---

## NAT Gateway

Allows resources in Private Subnets to access the Internet without being directly exposed.

Example

```text
Worker Node

↓

NAT Gateway

↓

Internet
```

Used for:

- Package Updates
- Docker Image Pulls
- API Calls

---

## Route Table

A Route Table decides where network traffic should go.

Example

```text
Destination          Target

0.0.0.0/0           Internet Gateway
```

or

```text
Destination          Target

0.0.0.0/0           NAT Gateway
```

---

## Security Group

Acts as a virtual firewall for an AWS resource.

Controls

- Inbound Traffic
- Outbound Traffic

Security Groups are Stateful.

---

## Network ACL (NACL)

Acts as a firewall for an entire subnet.

Controls

- Inbound Rules
- Outbound Rules

NACLs are Stateless.

---

# Public vs Private Subnets

## Public Subnet

Has a route to the Internet Gateway.

Typically contains

- ALB
- NAT Gateway
- Bastion Host

---

## Private Subnet

No direct Internet access.

Typically contains

- Kubernetes Worker Nodes
- Databases
- Internal Services

---

# Kubernetes Networking on AWS

Typical Architecture

```text
Internet

↓

Route53

↓

AWS ALB

↓

Public Subnet

↓

Target Group

↓

Worker Nodes

↓

Private Subnet

↓

Pods
```

---

# Production Example

Suppose users access

```text
https://shop.company.com
```

Traffic Flow

```text
User

↓

Route53

↓

AWS ALB (Public Subnet)

↓

Target Group

↓

Worker Node (Private Subnet)

↓

Service

↓

Pods

↓

Application
```

---

# Common Production Problems

## ALB Not Accessible

Possible Causes

- Internet Gateway Missing
- Wrong Route Table
- Security Group Blocking Port 80/443

---

## Worker Nodes Cannot Pull Images

Possible Causes

- NAT Gateway Missing
- Route Table Incorrect

---

## Pods Cannot Reach Internet

Possible Causes

- NAT Gateway Missing
- DNS Configuration Incorrect

---

## Database Not Reachable

Possible Causes

- Security Group Blocking Traffic
- Wrong Subnet
- Route Table Issue

---

# SRE Responsibilities

Monitor

- VPC Configuration
- Route Tables
- NAT Gateway
- Internet Gateway
- Security Groups
- Network ACLs
- Subnet Health

---

# Interview Questions

## What is a VPC?

A VPC is a logically isolated virtual network in AWS where AWS resources are deployed.

---

## What is the difference between Public and Private Subnets?

Public Subnets have a route to an Internet Gateway.

Private Subnets do not have direct Internet access.

---

## Why are Worker Nodes usually placed in Private Subnets?

For better security.

Only the Load Balancer is exposed to the Internet.

---

## What is the purpose of a NAT Gateway?

A NAT Gateway allows instances in Private Subnets to access the Internet without allowing inbound Internet connections.

---

## Difference between Security Group and NACL?

| Security Group | NACL |
|---------------|------|
| Instance Level | Subnet Level |
| Stateful | Stateless |
| Allow Rules Only | Allow and Deny Rules |

---

## What is the role of a Route Table?

A Route Table determines where network traffic should be forwarded.

---

# Key Takeaways

- VPC provides network isolation in AWS.
- Public Subnets host Internet-facing resources.
- Private Subnets host application workloads.
- Internet Gateway provides inbound and outbound Internet access.
- NAT Gateway provides outbound Internet access for Private Subnets.
- Security Groups protect individual resources.
- Network ACLs protect entire subnets.
- Most production Kubernetes clusters place ALBs in Public Subnets and Worker Nodes in Private Subnets.