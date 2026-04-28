# 🚀 دانلودر فایل ها درون گیتهاب شما

[![GitHub license](https://img.shields.io/github/license/yourusername/your-repo-name)](LICENSE)
[![GitHub release](https://img.shields.io/github/v/release/yourusername/your-repo-name)](https://github.com/yourusername/your-repo-name/releases)
[![Actions Status](https://github.com/yourusername/your-repo-name/workflows/AVASAM%20-%20Download%20from%20URLs%20&%20Save%20to%20Repo/badge.svg)](https://github.com/yourusername/your-repo-name/actions)

> **AVASAM** (https://avasam.ir) – Effortlessly download any file(s) from the web directly into your GitHub repository.  
> Supports splitting large files (>90MB) into zip‑compatible parts to stay under GitHub’s 100MB limit.

---

## ✨ Features

- 🔗 Download **multiple URLs** in a single run (space-separated)
- 📦 Choose between **normal** mode (keep individual files) or **zip** mode (bundle all into one archive)
- ✂️ Auto-split files larger than a threshold (default 90MB) – creates `.zip`, `.z01`, `.z02` … parts compatible with `zip -F`
- 🚀 Commits and pushes files in **batches of 5** to avoid API limits
- 🏷️ Branded with **AVASAM** – your download solution

---

## 📋 Prerequisites

- A GitHub account
- A repository (fork this one or create your own)

---

## 🔧 Setup Guide

### 1️⃣ Fork this repository

Click the **Fork** button (top right) to copy this repo to your GitHub account.

![Fork button](https://docs.github.com/assets/cb-20363/images/help/repository/fork_button.png)

### 2️⃣ Enable GitHub Actions write permissions

GitHub Actions need permission to push changes to your repository.  
Follow these steps:

- Go to your forked repository on GitHub
- Click **Settings** → **Actions** → **General**
- Under **Workflow permissions**, select **Read and write permissions**
- Click **Save**

![Workflow permissions](https://docs.github.com/assets/cb-23908/images/help/repository/actions-workflow-permissions.png)

> ⚠️ Without this, the action cannot commit and push downloaded files.

### 3️⃣ (Optional) Enable GitHub Pages for file access

If you want to serve downloaded files via a website, go to **Settings** → **Pages** → set source to `main` branch and `/docs` or `/(root)` – but this action saves files under `downloads/`, so you can link directly to raw.githubusercontent.com.

---

## 🎯 How to Use the Action

1. Go to the **Actions** tab of your forked repository.
2. Select **AVASAM - Download from URLs & Save to Repo** from the left sidebar.
3. Click **Run workflow**.
4. Fill in the inputs:
   - **urls** – Space-separated list of URLs to download  
     Example: `https://example.com/file1.zip https://example.com/file2.pdf`
   - **mode** – `normal` (keep files as separate) or `zip` (pack all into one archive)
   - **split_threshold_mb** – Files larger than this size (in MB) will be split into parts. Set `0` to disable splitting (but GitHub rejects files >100MB).
5. Click **Run workflow**.

![Run workflow](https://docs.github.com/assets/cb-25531/images/help/actions/workflow-dispatch-ui.png)

### Example inputs

| Input | Value |
|-------|-------|
| urls | `https://speed.hetzner.de/100MB.bin https://speed.hetzner.de/1GB.bin` |
| mode | `normal` |
| split_threshold_mb | `90` |

---

## 📂 Where do files go?

All downloaded files appear inside the `downloads/` folder of your repository after the workflow finishes.

- **Normal mode** → Each file is placed directly under `downloads/` (split multi-part `.zip`, `.z01`, … if size exceeded threshold).
- **Zip mode** → A single zip archive `archive_YYYYMMDD_HHMMSS.zip` containing all downloaded files.

---

## 🔁 How to reassemble split zip files

If a file was split (e.g., `largefile.zip`, `largefile.z01`, `largefile.z02`):

**Linux/macOS:**
```bash
zip -F largefile.zip --out combined.zip
unzip combined.zip
