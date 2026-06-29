# AWS Certificate Manager (ACM)

## What is AWS Certificate Manager (ACM)?

AWS Certificate Manager (ACM) is a managed AWS service that provisions, manages, and automatically renews SSL/TLS certificates.

These certificates are used to secure communication between clients and AWS services such as:

- Application Load Balancer (ALB)
- CloudFront
- API Gateway
- Elastic Beanstalk

---

# Why Do We Need ACM?

Without SSL/TLS

```text
User

↓

http://app.company.com

↓

Data Sent in Plain Text
```

Problems:

- Data can be intercepted
- Passwords are exposed
- Browser shows "Not Secure"

---

With ACM

```text
User

↓

https://app.company.com

↓

Encrypted Communication

↓

AWS ALB

↓

Application
```

Benefits:

- Secure Communication
- Automatic Certificate Renewal
- Browser Trust
- Data Encryption

---

# Architecture

```text
                User
                  │
                  ▼
       https://app.company.com
                  │
                  ▼
              Route53
                  │
                  ▼
            AWS ALB (HTTPS)
                  │
        ACM Certificate Attached
                  │
                  ▼
             HTTPS Listener
                  │
                  ▼
             Target Group
                  │
                  ▼
               Kubernetes
```

---

# How Does It Work?

## Step 1

User opens

```text
https://app.company.com
```

↓

## Step 2

Route53 resolves the domain.

↓

## Step 3

Request reaches the ALB.

↓

## Step 4

HTTPS Listener uses the ACM Certificate.

↓

## Step 5

SSL/TLS Handshake is completed.

↓

## Step 6

Encrypted communication starts.

↓

## Step 7

Traffic is forwarded to the Target Group.

---

# SSL/TLS Handshake

```text
Browser

↓

Client Hello

↓

ALB

↓

Server Hello

↓

Certificate (ACM)

↓

Certificate Validation

↓

Encrypted Session Created

↓

Secure Communication
```

---

# Certificate Types

## Public Certificate

Used for Internet-facing applications.

Example

```text
https://app.company.com
```

Issued by AWS ACM.

---

## Private Certificate

Issued using AWS Private CA.

Used for:

- Internal Applications
- Internal APIs
- Microservices

---

# Certificate Validation Methods

## DNS Validation

AWS provides a DNS record.

You add the record to Route53.

AWS verifies domain ownership automatically.

Recommended method.

---

## Email Validation

AWS sends a verification email to the domain owner.

Less commonly used.

---

# ACM Features

- Free Public SSL Certificates
- Automatic Renewal
- Managed by AWS
- Easy Integration with AWS Services
- Secure HTTPS Communication

---

# Production Example

```text
User

↓

https://shop.company.com

↓

Route53

↓

AWS ALB

↓

HTTPS Listener (443)

↓

ACM Certificate

↓

Target Group

↓

Ingress

↓

Service

↓

Pods
```

---

# Production Use Cases

- HTTPS Websites
- Kubernetes Applications
- API Gateway
- CloudFront
- Internal Applications

---

# Common Production Problems

## Certificate Expired

Possible Causes:

- Imported certificate not renewed
- Manual certificate management

---

## Browser Shows "Not Secure"

Possible Causes:

- No HTTPS Listener
- Wrong Certificate
- Expired Certificate

---

## Certificate Validation Failed

Possible Causes:

- DNS Validation Record Missing
- Wrong Hosted Zone
- Domain Not Verified

---

## HTTPS Not Working

Possible Causes:

- Listener configured on Port 80 only
- ACM Certificate not attached
- Wrong Security Group
- Incorrect DNS Record

---

# SRE Responsibilities

Monitor:

- Certificate Expiry
- HTTPS Availability
- SSL/TLS Errors
- Certificate Validation Status
- Listener Configuration

---

# Interview Questions

## What is AWS ACM?

AWS Certificate Manager is a managed service that provisions, manages, and automatically renews SSL/TLS certificates.

---

## Why do we use ACM?

To secure communication using HTTPS and simplify certificate management.

---

## What is the difference between HTTP and HTTPS?

HTTP sends data in plain text.

HTTPS encrypts communication using SSL/TLS.

---

## Where is the ACM Certificate attached?

Typically to:

- Application Load Balancer
- CloudFront
- API Gateway

For Kubernetes, it is commonly attached to the ALB HTTPS Listener.

---

## What is DNS Validation?

AWS verifies domain ownership by checking a DNS record in Route53.

---

## Does ACM automatically renew certificates?

Yes.

AWS-managed public certificates are automatically renewed before expiration.

---

## Can ACM certificates be downloaded?

No.

AWS-issued public ACM certificates cannot be exported.

(Exportable certificates are available only in specific ACM offerings.)

---

# Key Takeaways

- ACM manages SSL/TLS certificates.
- HTTPS provides encrypted communication.
- ACM integrates with ALB, CloudFront, and API Gateway.
- DNS Validation is the recommended validation method.
- AWS automatically renews public ACM certificates.
- ACM is commonly attached to the HTTPS Listener of an ALB.