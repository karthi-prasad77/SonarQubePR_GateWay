### Learning the SonarQube Code Analysis

# 📘 SonarQube CI/CD & PR Analysis – Learnings

## 🚀 Overview

This document summarizes the setup, issues, and learnings while integrating **SonarQube with GitHub Actions for .NET projects**, including CI/CD pipeline execution and Pull Request analysis.

---

## 🧠 What We Implemented

### 1. CI Pipeline using GitHub Actions

We created a workflow to:

* Checkout code
* Setup .NET 8
* Run SonarQube scanner
* Perform code analysis during build

---

### 2. SonarQube Scanner Integration

We used the following scanner command in GitHub Actions:

```bash
 dotnet sonarscanner begin \
  /k:"YOUR_PROJECT_KEY" \
  /d:sonar.host.url="${{ secrets.SONAR_HOST_URL }}" \
  /d:sonar.login="${{ secrets.SONAR_TOKEN }}" \
  /d:sonar.pullrequest.key=${{ github.event.number }} \
  /d:sonar.pullrequest.branch=${{ github.head_ref }} \
  /d:sonar.pullrequest.base=${{ github.base_ref }}
```

Followed by:

```bash
 dotnet build
 dotnet sonarscanner end /d:sonar.login="${{ secrets.SONAR_TOKEN }}"
```

---

## ⚠️ Issues Faced & Fixes

### ❌ 1. Authorization Error

**Error:**

> You're not authorized to run analysis

**Fix:**

* Generated correct token from SonarQube
* Ensured user has **Execute Analysis** permission
* Verified project access rights

---

### ❌ 2. PR Results Not Showing in GitHub

**Problem:**

* Build succeeded
* But no PR comments or decoration appeared

**Root Cause:**

* Missing or incorrect PR analysis parameters
* Or missing ALM integration

---

### ❌ 3. Missing PR Decoration Support

We learned that PR decoration depends on SonarQube edition:

| Edition           | PR Decoration     |
| ----------------- | ----------------- |
| Community Edition | ❌ Not Supported   |
| Developer Edition | ✅ Supported       |
| SonarCloud        | ✅ Fully Supported |

---

## 🔗 Key Concepts Learned

### 🟢 SonarQube

Used for code quality analysis, bug detection, and maintaining code standards.

---

### 🟢 GitHub Actions

Used to automate build, test, and SonarQube scanning process.

---

### 🟢 SonarCloud (Alternative)

A cloud-based version of SonarQube that supports PR decoration without self-hosting complexity.

---

## 🧩 What We Learned Overall

* How to integrate SonarQube with CI/CD pipelines
* How tokens and permissions control analysis access
* How PR analysis works internally
* Why PR decoration may fail
* Difference between SonarQube editions
* Importance of ALM integration for GitHub PR comments

---

## 🚀 Key Takeaway

> Build success ≠ PR visibility
> PR decoration depends on:

* Proper PR parameters
* ALM integration
* Correct SonarQube edition

---

## 📌 Next Steps

* Migrate to SonarCloud OR Developer Edition for PR comments
* Improve Quality Gate enforcement
* Add coverage reports integration
* Enhance CI pipeline with caching & faster scans
