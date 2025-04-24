---
title: "Understanding `.tfvars` in Terraform with Real EC2 Example"
date: 2025-04-12 00:00:00 +0000
layout: page
categories: [Terraform]
tags: [terraform-project] #devsecops, aws, best-practices
description: "A practical deep dive into `.tfvars` usage in Terraform, with EC2 deployment examples and industry-aligned best practices."
pin: false
math: false
toc: true
comments: true
---

## ✅ What I Learned Today

Today, I focused on one of the most fundamental, yet often misunderstood features in Terraform: **the use of `.tfvars` files**. I explored it with hands-on EC2 deployment and captured the Terraform behavior clearly.

---

## 🧠 Key Concepts Covered

- How `.tfvars` overrides `variables.tf`
- Real difference when changing EC2 `instance_type`
- When to use `.tfvars` and what to include in it
- Best practices to avoid pushing `.tfvars` to Git

---

## 🔨 What I Did

### 1️⃣ Initial Setup

I created the following files inside a Terraform project:

```hcl
# variables.tf
variable "instance_type" {
  default = "t2.micro"
}
```

```hcl
# terraform.tfvars
instance_type = "t3.micro"
```

```hcl
# main.tf
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
  ...
}
```

---

### 2️⃣ Observing Terraform Behavior

When I ran `terraform plan`, it showed:

```
~ instance_type = "t2.micro" -> "t3.micro"
-/+ will be replaced
```

🧠 **Meaning**: EC2 instance is immutable on `instance_type` change, so Terraform will destroy and recreate it.

---

### 3️⃣ Real-World Learning

- Restarting EC2 manually in the AWS Console causes Terraform to detect public IP changes — even without code changes.
- If `.tfvars` has values, Terraform always uses them instead of `variables.tf` defaults.
- **Variable Precedence Order** (High → Low):

```
CLI -var flag
> CLI -var-file flag
> terraform.tfvars file
> *.auto.tfvars files
> Environment variables
> Default in variables.tf
```

---

### 📁 Folder Structure

```
tf-infra-ec2/
├── main.tf
├── variables.tf
├── terraform.tfvars
├── outputs.tf
├── terraform.tfstate
```

---

## ✅ Best Practices I Followed

- ✅ Keep `.tfvars` local and do not push to Git
- ✅ Use `.tfvars.example` in repo for team reference
- ✅ Override environment-specific values only in `.tfvars`
- ✅ Use `terraform plan -var-file="dev.tfvars"` in multi-env setups

---

## 🧪 Key Commands Used

- `terraform init`
- `terraform plan -out=plan.out`
- `terraform apply "plan.out"`
- `terraform show`

---

## 📚 My Takeaway

Understanding `.tfvars` gives you confidence in clean separation of code vs config in Terraform.  
It’s a best practice that ensures clarity, reusability, and secure deployments in real teams and environments.

---

*Thanks for reading. May your pipelines be green, your infra be tagged, and your security posture be strong!*

🔗 Explore my projects, policies, and pipelines on [opsbygandal.dev](https://www.opsbygandal.dev)  
📁 Browse my GitHub repositories: [github.com/gandalops](https://github.com/gandalops?tab=repositories)  
🔄 Feedback? Let’s connect on [LinkedIn](https://www.linkedin.com/)

---
