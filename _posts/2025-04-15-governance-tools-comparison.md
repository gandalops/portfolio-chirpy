---
title: "Governance & Policy as Code: My DevSecOps Operating System"
date: 2025-04-15 00:00:00 +0000
last_modified_at: 2025-04-15 00:00:00 +0000
categories: [blog]
tags: [terraform, opa, tfsec, governance, devsecops, policy-as-code]
excerpt: "How I apply real-world governance rules, policy enforcement, and security practices across all 21 DevSecOps Epics."
pin: false
math: false
toc: true
comments: true
---

# 🔐 DevSecOps Governance Tools Comparison

As a DevSecOps practitioner, it's critical to choose the right Policy-as-Code tool based on your stack and use case. Below is a comparison of 5 key tools used to enforce governance across CI/CD pipelines, Infrastructure as Code, and Kubernetes.

---

## 📊 Tool Overview Table

| Tool       | Ecosystem Fit                 | Policy Language     | Best For                              | Realtime Enforcement | Difficulty |
|------------|-------------------------------|---------------------|----------------------------------------|-----------------------|------------|
| OPA        | Cloud-native (K8s, Terraform) | Rego                | Generic policy engine, APIs, Terraform | ✅ Yes (via Gatekeeper) | ⚙️ Medium |
| Sentinel   | HashiCorp Stack               | Sentinel (HCL-like) | Terraform Enterprise, Vault, Consul    | ✅ Yes                 | ⚙️ Medium |
| Checkov    | Terraform, CloudFormation     | YAML + Python       | IaC static security checks             | ❌ No (pre-apply only) | 🟢 Easy   |
| Conftest   | YAML, JSON, Dockerfiles, etc. | Rego (OPA)          | CI/CD config validation                | ❌ No (CI only)        | ⚙️ Medium |
| Kyverno    | Kubernetes-native             | Declarative YAML    | Kubernetes admission policies          | ✅ Yes                 | 🟢 Easy   |

---

## 🛠 Real-World Use Cases

### ✅ OPA (Open Policy Agent)

- Enforce no unencrypted S3 buckets in Terraform  
- Block Kubernetes pods from running as root (via Gatekeeper)  
- Apply policies on custom APIs (Envoy/NGINX integration)  

```rego
# Disallow root containers in Kubernetes
violation[{"msg": msg}] {
  input.spec.securityContext.runAsNonRoot == false
  msg := "Containers must not run as root"
}
```

---

### ✅ Sentinel

- Block Terraform plans that don't tag resources  
- Ensure Vault secrets are scoped properly  
- Enforce job-level access policies in Nomad  

```hcl
# Enforce tagging policy
main = rule { all resources.r as r { r.tags contains "env" } }
```

---

### ✅ Checkov

- Scan Terraform for open security groups  
- Ensure RDS has backup retention  
- Detect hardcoded secrets  

```yaml
# Checkov finding example
check: CKV_AWS_24
message: Ensure no open security group to 0.0.0.0/0
```

---

### ✅ Conftest

- Enforce variables in Ansible playbooks  
- Validate GitHub Actions workflows  
- Run pre-merge config checks in CI  

```rego
# Required env variable must exist
deny[msg] {
  not input.env
  msg := "Missing required env key"
}
```

---

### ✅ Kyverno

- Block use of `latest` image tag  
- Auto-label all pods with team info  
- Enforce CPU/memory limits  

```yaml
# Block latest tag in image
spec:
  validation:
    message: "Avoid using latest tag"
    pattern:
      spec:
        containers:
        - image: "!*:latest"
```

---

## 🧠 Summary: How to Choose

| Scenario                                | Start With     |
|-----------------------------------------|----------------|
| General-purpose policies                | OPA            |
| You use Terraform Enterprise            | Sentinel       |
| Quick IaC scan before deploying         | Checkov        |
| Need portable CI/CD policy checks       | Conftest       |
| Kubernetes native enforcement           | Kyverno        |

---

## 🔗 Explore More

- 🌐 [openpolicyagent.org](https://www.openpolicyagent.org/)  
- 🌐 [developer.hashicorp.com/sentinel](https://developer.hashicorp.com/sentinel)  
- 🌐 [checkov.io](https://www.checkov.io/)  
- 🌐 [conftest.dev](https://www.conftest.dev/)  
- 🌐 [kyverno.io](https://kyverno.io/)

---

*Thanks for reading. May your pipelines be green, your infra be tagged, and your security posture be strong!*
🔗 Explore my projects, policies, and pipelines on [opsbygandal.dev](https://www.opsbygandal.dev)  
📁 Browse my GitHub repositories: [github.com/gandalops](https://github.com/gandalops?tab=repositories)  
🔄 Feedback? Let’s connect on [LinkedIn](https://www.linkedin.com/)

---

