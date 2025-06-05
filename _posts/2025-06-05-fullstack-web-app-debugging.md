---
title: "How I Debugged My First Web App Locally – A DevOps Beginner’s Journey"
date: 2025-06-05 00:00:00 +0000
last_modified_at: 2025-06-05 00:00:00 +0000
categories: [blog]
tags: [springboot, reactjs, mysql, jwt, devops, debugging]
excerpt: "How I went from 401 Unauthorized to a working login flow. A beginner's breakdown of debugging a full-stack expense tracker with Spring Boot, JWT, React, and MySQL."
pin: false
math: false
toc: true
comments: true
---

# 🛠️ How I Debugged My First Web App Locally – A DevOps Beginner’s Journey

Launching your first full-stack web application is a thrilling yet frustrating experience — and I lived through that rollercoaster! This blog shares the gritty details of how I debugged my first **Spring Boot + React + MySQL** app and finally saw that magical *“Dashboard”* appear on screen.

---

## 🌐 What Is This Web App?

The app is a **Full-Stack Expense Tracker** called **“My Wallet”**, which helps users:

* Track expenses & incomes  
* Set budgets and categories  
* Register, log in, and securely manage their finances

---

## ⚙️ Tech Stack Overview

| Layer    | Technology Used                                           |
| -------- | --------------------------------------------------------- |
| Frontend | React.js (Port: `3000`)                                   |
| Backend  | Spring Boot (Java) + Spring Security + JWT (Port: `8081`) |
| Database | MySQL (Local, DB name: `expensetrackerdb`)                |

---

## 🧩 My Initial Setup Flow

### ✅ Step 1: Start Backend

```bash
nohup ./mvnw spring-boot:run > backend.log 2>&1 &
````

📉 **Error #1:**
`Access denied for user 'root'@'localhost'`
🔍 Root Cause: MySQL password mismatch in `application.properties`.

✅ **Fix:**

* Confirmed correct MySQL password with `mysql -u root -p`
* Updated backend config with correct creds
* Restarted backend

---

### ❌ Error #2: Port 8080 Already in Use

✅ Solution: Changed backend port to `8081` by adding `server.port=8081` in `application.properties`.

---

### ✅ Step 2: Start Frontend

```bash
nohup npm install > frontend.log 2>&1 &
nohup npm start > frontend.log 2>&1 &
```

I could now see the login UI 🎉 — but logging in didn’t work.

---

## 🔐 The JWT Nightmare

I spent hours stuck on:

```
401 Unauthorized – Full authentication is required to access this resource
```

Turns out, my **JWT token was never being generated**.

### ✅ The Root Cause?

A **Java mistake**:
I had written the logger *after* the `return` line — so token generation logic was silently skipping the log.

**Original Code:**

```java
return Jwts.builder()....compact();
logger.info("Generated JWT Token: " + token); // ❌ Never executes
```

**Fixed Code:**

```java
String token = Jwts.builder()....compact();
logger.info("Generated JWT Token: {}", token);  // ✅ Now logs
return token;
```

---

## 🧪 BCrypt Password Hashing

Login was still failing due to this:

```
401 Unauthorized – Bad credentials
```

🔍 My manually inserted admin password didn’t match the expected **BCrypt format**.

✅ I generated a valid hashed password using:

```java
System.out.println(new BCryptPasswordEncoder().encode("Admin@123"));
```

🔁 Updated the DB manually:

```sql
UPDATE users SET password = 'hashed_value_here' WHERE email = 'admin2@example.com';
```

---

## ✅ Final Login Success!

Ran this curl test:

```bash
curl -X POST http://localhost:8081/mywallet/auth/signin \
  -H "Content-Type: application/json" \
  -d '{"email": "admin2@example.com", "password": "Admin@123"}'
```

🎉 Got the JWT Token!
And YES — logged into the UI from `http://localhost:3000` for the first time! 🎯

---

## 🎯 What I Learned

| Problem                     | What I Did                        |
| --------------------------- | --------------------------------- |
| MySQL Access Denied         | Verified password, updated config |
| Port Conflict               | Switched backend to port 8081     |
| JWT Token Not Generated     | Fixed logger placement in Java    |
| Password Not Matching (401) | Used BCrypt and updated DB        |
| UI Login Not Working        | Enabled JWT + Spring Security     |

---

## 🧠 Final Reflection

Debugging this app taught me more than any tutorial ever could:

* **Security is real** — every login needs backend verification.
* **DevOps starts with Dev** — I couldn’t debug CI/CD until I made local work.
* **Logs are gold** — every issue had a hint hiding in `backend.log` & `frontend.log`.
* **Shell scripts usage** — I created run.sh and stop.sh to start both frontend and backend together and gracefully release ports when stopping. This saved time and reduced manual mistakes during repeated test cycles.
* **Automation is empowerment** — even local DevOps benefits from reusable scripts and patterns that reduce cognitive load.

---

If you’re stuck in your first full-stack app journey: don’t quit.
**Break it down. Fix one log error at a time. Victory is near.**

*Thanks for reading. May your pipelines be green, your infra be tagged, and your security posture be strong!*

🔗 Explore my other blogs at [opsbygandal.dev](https://www.opsbygandal.dev)  
📁 Check out the GitHub repo [https://github.com/gandalops/expense-tracker]  
🔄 Let’s connect on [LinkedIn](https://www.linkedin.com/)


```

