---
title: "From Minimal Mistakes to Chirpy: Why I Switched and Never Looked Back"
date: 2025-04-25 00:00:00 +0000
categories: [blog]
tags: [jekyll, minimal-mistakes, chirpy, devcontainer, ruby, productivity, devsecops, portfolio]
excerpt: "My week-long struggle with customizing Minimal Mistakes made me realize I'm here to build DevSecOps pipelines, not frontend layouts. Chirpy gave me clarity, speed, and peace of mind."
pin: false
math: false
toc: true
comments: true
---

> — Me, after one week of customizing Minimal Mistakes

## 🔧 The Initial Setup: Minimal Mistakes Looked Promising…

Like many developers building a personal portfolio, I started with the popular [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) Jekyll theme. It’s feature-rich, clean, and highly customizable.

But soon after, I hit the wall.

## 😓 What Went Wrong with Minimal Mistakes

Here’s what I struggled with:

### 1. 🧩 Too Much Customization Needed
- Wanted a homepage with sections like About, Projects, Blog, and Contact?  
  → Had to customize `_includes`, `_layouts`, and write custom CSS.
- Any layout tweak broke something else—like the sidebar or ToC.

### 2. 🧪 Front-End Complexity
- I’m not a frontend expert. I work in **DevSecOps**, and while I understand HTML/CSS basics, I don’t enjoy tweaking JS libraries or layout files.
- Customizing layouts like `single.html` or adding a TOC on the right sidebar took hours.

### 3. 💥 Frequent Compatibility Issues
- Theme updates sometimes conflicted with my setup.
- Ruby and gem dependency mismatches kept breaking local builds.

### 4. ⌛ Time Drain
- Spent **more time debugging UI** than writing blog content or sharing DevOps learnings.
- Every visual enhancement became a task.

## 🚀 The Switch: Discovering Chirpy

I told ChatGPT:
> _“I want a clean and professional website to showcase my DevSecOps work. I don’t want to waste time on design—I just want to focus on writing blogs and showing projects.”_

That’s when I found [**Chirpy**](https://chirpy.cotes.page).

Within a day, I:
- Migrated all blog posts ✅  
- Set up a tag-based blog index ✅  
- Customized logo, sidebar, footer ✅  
- Hosted the site on GitHub Pages ✅  

It was refreshingly simple.

## ✅ What Chirpy Got Right

| Feature                      | Minimal Mistakes       | **Chirpy** ✅                  |
|-----------------------------|------------------------|-------------------------------|
| Out-of-the-box layout       | Basic, needs tweaking  | Clean, minimal, blog-focused  |
| TOC support                 | Custom include needed  | Built-in with sticky sidebar  |
| Tags & Categories           | Manual + layout setup  | Automatically shown per post  |
| Footer & Metadata           | Manual editing         | Pre-configured & SEO-ready    |
| Deployment on GitHub Pages  | Works well             | Same, zero extra config       |
| Responsive Design           | Needs effort           | Already polished              |
| Speed of Setup              | Took days              | **Setup in a day**            |

## 🐳 Bonus: Setting Up a DevContainer Was a Game Changer

While working on this transition, I wanted to **keep my system clean** and not mess around with local Ruby versions or Jekyll dependencies. That’s where **DevContainers** came to the rescue.

### 🛠️ How I Did It:
- I used **VS Code Dev Containers** with the official `jekyll/jekyll` Docker image.
- All I needed was a `.devcontainer/devcontainer.json` and a Dockerfile.
- Ruby, Bundler, and all Jekyll dependencies were **pre-installed** inside the container.
- I mounted my project inside it and instantly had a clean, reproducible environment.

### 💡 Why It Was Worth It:
- 🔄 **No Ruby on my host machine**—no version conflicts, no gem clutter.
- 🧼 **Isolated Dev Environment**—I can delete or rebuild my container anytime.
- ⚙️ **Consistent Builds**—What works in the container will work on CI or another laptop.
- 🐳 **Docker-powered DevOps discipline**—exactly the mindset I’m growing in.

So even if Chirpy was simpler, having it **inside a DevContainer** made the entire experience smoother, cleaner, and more DevSecOps-aligned.

## 🧠 Final Thoughts

After customizing Minimal Mistakes for **one full week**, I realized:  

Chirpy helped me shift my focus back to **content** and **learning**.  
The DevContainer made sure I didn’t waste time setting up dependencies again and again.

If you're a DevOps/Cloud/Tech learner looking to showcase your work without UI headaches—  
**Just go with Chirpy.** You’ll thank me later.

*Thanks for reading. May you have an easy, quick, and joyful portfolio setup experience with Chirpy.!*

🔗 Explore my other blogs at [opsbygandal.dev](https://www.opsbygandal.dev)  
📁 Check out the GitHub repo [https://github.com/gandalops/portfolio-chirpy]  
🔄 Let’s connect on [LinkedIn](https://www.linkedin.com/)