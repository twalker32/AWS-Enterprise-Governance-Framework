# SCP Examples

This folder contains foundational Service Control Policies (SCPs) used to enforce preventive security controls across AWS Organizations.

---

## 📋 Included Policies

- **`deny-unapproved-regions.json`**  
  Blocks all AWS regions except those explicitly allowed (e.g., `us-east-1`, `us-west-2`).

- **`enforce-s3-encryption.json`**  
  Denies the `s3:PutObject` action unless server-side encryption is specified, ensuring all data written to S3 is protected.

- **`deny-iam-escalation.json`**  
  Prevents IAM users or roles from attaching, creating, or escalating privileges through IAM policy manipulation.

---

## 🎯 Purpose

These SCPs form the preventive layer of the AWS governance model. When applied at the OU or root level, they reduce misconfigurations, enforce least privilege, and protect critical data pathways at scale.

Used in combination with AWS Config, Security Hub, and delegated admin models for IAM, these SCPs help drive a **secure, compliant AWS foundation.**
