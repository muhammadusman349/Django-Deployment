# 🚀 Django Railway Deployment Guide

This guide explains how to deploy a **Django project** to [Railway](https://railway.app) using GitHub integration.

---

## 📂 Project Setup Files

Before deploying, ensure your repository contains the following essential files:

### 1️⃣ `conf/settings.py`
- Configure **`ALLOWED_HOSTS`** to include Railway's domain.
- Set database configuration for production.
- Example:
```python
ALLOWED_HOSTS = ["your-railway-domain.up.railway.app", "localhost"]
2️⃣ Procfile
Defines the process to run your Django app:
web: gunicorn your_project.wsgi

3️⃣ Aptfile
List of Ubuntu packages needed:
python3-dev
libpq-dev

4️⃣ runtime.txt
Specifies the Python version:
python-3.9.13

5️⃣ build_files.sh (optional)
Custom build script:
pip install -r requirements.txt
python manage.py collectstatic --noinput
python manage.py migrate

# Make it executable:
chmod +x build_files.sh

## 🛠 Railway Deployment Steps
Step 1: Prepare Your Repository
Commit all required config files (Procfile, runtime.txt, etc.).

** Do NOT commit .env — store sensitive variables in Railway dashboard.

### Step 2: Create a Railway Project
Sign in to Railway.

Click "New Project" → "Deploy from GitHub repo".

Select your repository.

###Step 3: Configure Environment Variables
Go to Project → Variables.

Add:

SECRET_KEY → Your Django secret key.

DEBUG → False.

DATABASE_URL → Provided by Railway when using PostgreSQL.

Any other API keys your project needs.

### Step 4: Configure Deployment Settings
Set Root Directory if your project is not in the root.

Set Build Command (if using build_files.sh):
bash build_files.sh
Ensure Start Command is from your Procfile.

### Step 5: Deploy
Railway deploys automatically on push to the linked branch.

Monitor logs in Deployments and Logs tabs.

### Step 6: Database Setup
If using PostgreSQL:
railway run python manage.py migrate
✅ Post-Deployment
Access your app via the Railway-provided domain.

Add a custom domain in Settings.

Enable automatic deployments from a specific branch.

🐛 Troubleshooting
Build fails? → Check logs for missing dependencies.

Static files not loading? → Ensure collectstatic is run and Whitenoise is set up.

Database errors? → Verify DATABASE_URL and run migrations.

📌 Example .env (for local dev)

SECRET_KEY=your_secret_key
DEBUG=True
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname
📖 References

Django Deployment Checklist
Railway Docs

Author:Muhammad Usman
License: Unlicense