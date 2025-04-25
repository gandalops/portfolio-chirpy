---
title: "How I Brought My Broken DevSecOps Blog Back to Life – A Jekyll, Ruby & GitHub Pages Debugging Journey"
date: 2025-04-14 00:00:00 +0000
last_modified_at: 2025-04-15 00:00:00 +0000
categories: [blog]
tags: [jekyll, ruby, github-pages, minimal-mistakes, debugging, errors]
description: "My debugging journey fixing broken builds caused by Ruby version mismatches, unsupported Jekyll plugins, and GitHub Pages limitations — with real errors and fixes."
pin: false
math: false
toc: true
comments: true
---

Building my DevSecOps portfolio site was a moment of excitement — until it broke. 😅 What started as a clean Jekyll + GitHub Pages setup turned into a deep dive into Ruby versions, unsupported plugins, CI/CD failures, and more. Here’s how I fixed it all, and what I learned in the process.

---

## 🧠 What This Blog Covers
- Ruby version mismatches and GitHub Actions errors
- Plugin compatibility with GitHub Pages
- Liquid template errors (`include_cached`)
- Cleanup strategies to get a clean build
- Importance of building locally before pushing to remote
- How I learned to stay calm and debug methodically
- Reading CI/CD logs carefully and spotting simple config fixes

---

## 🔥 1. Error: `Unknown tag 'include_cached'`

**What Happened:**  
Jekyll build failed with a Liquid syntax error because the theme was using the `{% raw %}{% include_cached %}{% endraw %}` tag.

**Root Cause:**  
`jekyll-include-cache` is **not supported** by GitHub Pages. The newer Minimal Mistakes versions depend on it.

**Fix:**
- Downgraded `minimal-mistakes-jekyll` to version `4.22.0`
- Removed `jekyll-include-cache` from `Gemfile` and `_config.yml`

**Preserving the Example:**
To keep a reference without breaking the build:

````liquid
{% raw %}
{% include_cached somefile.html %}
{% endraw %}
````

---

## 🧱 2. Error: Ruby Version Mismatch (3.1.7 vs 3.2.3)

**What Happened:**  
GitHub Actions failed with:
> Your Ruby version is 3.1.7, but your Gemfile specified ~> 3.2.3

**Fix:**
- Updated `.ruby-version` to `3.1.4`
- Regenerated `Gemfile.lock`
- Synced Ruby version in GitHub Actions config

---

## 🧼 3. Error: Bundle Platform Error

**What Happened:**  
> Your bundle only supports platforms [...] but your local platform is ruby

**Fix:**
```bash
bundle lock --add-platform ruby
```

---

## 💣 4. Cache Conflicts and Native Extension Errors

**Fix:**  
Created a `cleanup.sh`:

```bash
#!/bin/bash
rm -rf _site/ .jekyll-cache/ .jekyll-metadata .bundle/ vendor/ Gemfile.lock
bundle clean --force
echo "✅ Clean slate!"
```

---

## 🚧 5. Git Push Syntax Error

**What Happened:**
```bash
git push add origin
```

**Fix:**
```bash
git push origin main
```

---

## 🔧 6. Actions Version Mismatch in Workflow Config

**What Happened:**
```yaml
- uses: actions/upload-pages-artifact@v2
- uses: actions/deploy-pages@v2
```

**Fix:**
```yaml
- uses: actions/upload-pages-artifact@v3
- uses: actions/deploy-pages@v4
```

**Lesson:** Always check for latest GitHub Actions versions.

---

## 🧠 Lessons I Learned

- 🔍 **Slow down** — most bugs were config/environment related
- 🧱 Match Ruby + Gemfile + GitHub Pages versions
- 🔁 Lock versions with `bundle lock`
- 🧪 Build **locally** first
- 💡 Read logs line by line — answers are usually there!

---

## 🛠️ Bonus: My Maintenance Script

```bash
#!/bin/bash
# Safe cleanup
rm -rf _site/ .jekyll-cache/ .jekyll-metadata .bundle/ vendor/ Gemfile.lock
bundle clean --force
echo "Ready to rebuild: bundle install && bundle exec jekyll serve"
```

---

If you're debugging your Jekyll site today — trust me, you're not alone.  
The key is to align your environment, match versions, and read the logs patiently.

---

*Thanks for reading. May your pipelines be green, your infra be tagged, and your security posture be strong!*

🔗 Explore my other blogs at [opsbygandal.dev](https://www.opsbygandal.dev)  
📁 Check out the GitHub repo [https://github.com/gandalops/portfolio-chirpy]  
🔄 Let’s connect on [LinkedIn](https://www.linkedin.com/)

---

