
# 🛡️ Identity and Access Governance Guide

This guide outlines how large enterprises like Toyota can securely manage access across AWS accounts using scalable and auditable practices.

---

## 🎯 Goals

- Centralized identity and access control across all accounts
- Minimized blast radius via permission boundaries
- Federation with enterprise identity providers (IdPs)
- Support for attribute-based access control (ABAC)
- Prevention of privilege creep over time

---

## 1️⃣ Federation and Identity Center

Use **IAM Identity Center (formerly AWS SSO)** to connect your corporate IdP (Okta, Azure AD, etc.) with AWS.

Benefits:
- Centralized user/group lifecycle
- Assign roles via groups (e.g., `Developers`, `CloudAdmins`)
- Reduce reliance on long-lived IAM users

📌 Tip: Assign **Permission Sets** that map to job functions and are scoped via least privilege.

---

## 2️⃣ Permission Boundaries and Guardrails

Use **permission boundaries** for developers and automation roles. These act as a "maximum access cap" — even if a role has `AdministratorAccess`, it will be limited by the boundary.

Use **SCPs** from the Organizations layer to restrict dangerous actions globally (e.g., `iam:PassRole`, `ec2:TerminateInstances` in production).

---

## 3️⃣ Cross-Account Role Access (Zone of Trust)

Define a secure trust zone across accounts.

- Allow `AssumeRole` only from designated identities
- Use external IDs or condition tags for extra safety
- Minimize use of wildcard `Principal` in trust policies

📌 Example: Grant the central DevOps account access to deploy into dev/staging/prod using scoped roles and `Condition` blocks on tags like `Environment`.

---

## 4️⃣ Attribute-Based Access Control (ABAC)

Use **tags** (e.g., `project:alpha`, `team:data`) on roles and resources to enable scalable ABAC.

Benefits:
- No need to manage dozens of custom IAM policies
- Dynamically grant access based on attributes
- Great for multi-team, multi-project environments

🔐 Example:
```json
"Condition": {
  "StringEquals": {
    "aws:ResourceTag/project": "${aws:PrincipalTag/project}"
  }
}
```

---

## 5️⃣ Role Lifecycle Management

- Implement scheduled **IAM Access Analyzer** scans to detect over-privileged or unused roles
- Use **Access Advisor** data to clean up unused permissions
- Rotate cross-account roles and access credentials regularly

---

## 🔁 Recap

| Capability | AWS Service / Tool |
|------------|---------------------|
| Federation | IAM Identity Center |
| Least Privilege | SCPs + Permission Boundaries |
| ABAC | Tag-based policies |
| Auditing | IAM Access Analyzer + Access Advisor |
| Cross-account | `AssumeRole` + Scoped Trust Policies |

---

## 📚 Helpful Links

- [IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)
- [AWS ABAC Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)
- [Permission Boundaries](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)
- [AWS Organizations SCPs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)

---

This identity governance model supports rapid scaling while keeping access safe, auditable, and aligned with Toyota’s enterprise standards.
