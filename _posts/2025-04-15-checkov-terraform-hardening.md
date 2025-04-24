---
title: "Securing Terraform with Checkov: My First Governance Hardening Pass"
date: 2025-04-15 00:00:00 +0000
last_modified_at: 2025-04-15 00:00:00 +0000
categories: [DevSecOps, Governance]
tags: [terraform, checkov, devsecops, security, governance, policy-as-code]
excerpt: "My first hands-on experience applying security policies to Terraform using Checkov. From 11 failed checks to just 3, here’s what I learned."
pin: false
math: false
toc: true
comments: true
---

🔍 **Context:** As part of my DevSecOps learning journey (Epic 21: Governance & Policy as Code), I wanted to enforce security best practices in a live Terraform project. The goal was to use Checkov to scan my Infrastructure as Code (IaC) and apply necessary fixes to improve compliance, visibility, and confidence in what I was deploying.

---

## 🧱 Scenario Before Scan

I began with a basic Terraform setup that provisioned:

- A VPC  
- A public subnet  
- A security group allowing SSH  
- A single EC2 instance with default settings  

---

## 🚨 Initial Risks Detected by Checkov

```bash
checkov -d .
```

**Results:**

- ✅ Passed Checks: 9  
- ❌ Failed Checks: 11  

**Examples:**

- SSH open to `0.0.0.0/0`  
- Egress open to all traffic  
- No `description` fields  
- EBS not encrypted  
- No CloudWatch monitoring  
- IMDSv1 enabled  
- No IAM role for EC2  
- Public IP assigned automatically  
- VPC Flow Logs disabled  

---

## 🔧 Fixes Made (Batch-Wise)

### 🔐 Batch 1: Security Group Tightening

- Allowed SSH only from trusted IP  
- Restricted egress to port 443 (HTTPS)  
- Added `description` fields to rules  

---

### 💾 Batch 2: EC2 Instance Hardening

- Enabled CloudWatch monitoring  
- Encrypted EBS root volume  
- Enforced IMDSv2  
- EBS optimization enabled  
- Commented `iam_instance_profile` (will add later)  

---

### 🌐 Batch 3: Network & VPC Adjustments

- Disabled `map_public_ip_on_launch`  
- Deferred VPC Flow Logs setup  

---

## 🧪 Terraform Workflow Used

```bash
terraform plan -out secure.plan
terraform apply secure.plan
```

✅ Avoided blind changes — DevSecOps habit formed!

---

## 📊 After Fixes: Checkov Re-scan

```bash
checkov -d .
```

**Result:**

- ✅ Passed: 17  
- ❌ Failed: 3 (deferred by design)  

Remaining issues:

- IAM role missing → planned in Task 21.5  
- VPC flow logs → Task 21.7  
- Default security group → not used  

---

## 🧠 Key Learnings

- Policy-as-Code reveals hidden risks  
- IMDSv2 + encryption = huge upgrade  
- Organizing fixes into batches = clarity  
- `terraform plan -out` = must-use  
- DevSecOps = awareness + tooling + design  

---

## 📝 What’s Next?

- Write custom OPA policy for EBS encryption  
- Checkov scan for S3 bucket policies  
- Add IAM role with least privilege  
- Enable VPC Flow Logs with IAM  
- Blog each fix as a security pattern  

---

*Thanks for reading. May your pipelines be green, your infra be tagged, and your security posture be strong!*  
🔗 Explore my projects, policies, and pipelines on [opsbygandal.dev](https://www.opsbygandal.dev)  
📁 Browse my GitHub repositories: [github.com/gandalops](https://github.com/gandalops?tab=repositories)  
🔄 Feedback? Let’s connect on [LinkedIn](https://www.linkedin.com/)
```

---

