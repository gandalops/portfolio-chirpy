---
title: "Main vs gh-pages: Lessons from Setting Up Chirpy and Understanding Real CI/CD"
date: 2025-04-26
categories: [blog]
tags: [github, chirpy, ci-cd, devops]
excerpt: "Lessons learned while correctly separating source and production branches, both for static websites like Chirpy and real-world CI/CD deployments."
pin: false
math: false
toc: true
comments: true
---

# Main vs gh-pages: Lessons from Setting Up Chirpy and Understanding Real CI/CD

Recently, while setting up my portfolio site using the Chirpy Jekyll theme and GitHub Actions, I faced an important workflow issue. Through this experience, I learned a fundamental concept that applies not only to building static websites, but also to real-world CI/CD pipelines.

Here are my combined learnings, structured for easy recall.

---

## 📚 What You Should Take from This Incident

### 1. `main` (or `master`) is for Source Code
Always keep the following in `main`:
- `_config.yml`
- `index.html`
- `_posts/`, `_layouts/`, `_includes/`
- Scripts, workflows, and all editable project files

You edit, update, and manage your project inside the `main` branch.

### 2. `gh-pages` is for Built Static Output
The `gh-pages` branch should contain only:
- Static HTML
- CSS, JS, Images
- Ready-to-serve files generated by a build process

You never manually edit `gh-pages`. It is the final product served by GitHub Pages.

### 🔄 Easy Rule to Remember
| Task | Use `main` or `gh-pages`? |
|:---|:---|
| Writing, updating, designing | `main` |
| Building and serving | `gh-pages` |

### 🧱 Mental Map
```
[main] --> GitHub Action builds --> [gh-pages] --> GitHub Pages serves website
```
You work on `main`, GitHub Actions build it, and `gh-pages` serves it.

---

## 🎯 In Any CI/CD Pipeline (Including GitHub Actions)

The same principle applies even in larger CI/CD systems for applications, APIs, cloud infrastructure, etc.

| Purpose | What Happens | Branch |
|:---|:---|:---|
| Source Code | Developers push code | `main`, `develop`, or feature branches |
| Build / Test | GitHub Actions or Jenkins builds and tests automatically | triggered from `main` or PR |
| Deploy | GitHub Actions deploy final product (artifact, website, infra) | deploy to servers/cloud |

You never directly edit the production output manually.

### 💥 Real-World CI/CD Examples
- **Java App**: Push code to `main` -> GitHub Actions build JAR -> Dockerize -> Deploy to AWS ECS.
- **React Website**: Push code to `main` -> Build static site -> Deploy to S3 + CloudFront.
- **Terraform Infrastructure**: Push .tf files to `main` -> Validate and apply infrastructure changes automatically.


### 📊 Professional Deployment Flow
```
Developer pushes to [main]
          ↓
GitHub Actions pipeline triggers build/test
          ↓
If successful, deploys to production target
```

---

# 📊 Summary Checklist

| Context | Main Branch | Production (Output) |
|:---|:---|:---|
| Jekyll/Chirpy Site | Source code (`main`) | Built site (`gh-pages`) |
| Java App CI/CD | Source code (`main`) | Docker image/server deployment |
| Terraform Infra | Source code (`main`) | Applied cloud infra (AWS, Azure) |

**Golden Rule:**
> Work on `main`. Let CI/CD or GitHub Actions build and push the result to production targets.


---

This learning has changed the way I structure not just static sites, but also application and infrastructure projects. A simple mistake opened a much bigger door to understanding real CI/CD workflows!

*Thanks for reading. Wishing you a smooth and professional journey in mastering GitHub Actions, branch workflows, and CI/CD pipelines!*

🔗 Explore my other blogs at [opsbygandal.dev](https://www.opsbygandal.dev)  
📁 Check out the GitHub repo [portfolio-chirpy](https://github.com/gandalops/portfolio-chirpy)  
🔄 Let’s connect on [LinkedIn](https://www.linkedin.com/)

