# Quick Reference: GitHub Push Permission Fix

## The Error
```
You don't have permissions to push to "mauriciotcec-dev/backend-data" on GitHub.
Would you like to create a fork and push to it instead?
```

## Quick Solutions

### 🍴 Solution 1: Fork (Fastest for Contributors)
```bash
# 1. Click "Fork" on GitHub
# 2. Clone your fork
git clone https://github.com/YOUR_USERNAME/backend-data.git
cd backend-data

# 3. Make changes, commit, and push
git checkout -b my-feature
git add .
git commit -m "My changes"
git push origin my-feature

# 4. Create Pull Request on GitHub
```

### 🔑 Solution 2: Fix Authentication (If You Have Access)

**Option A: Personal Access Token**
```bash
# Generate token at: github.com → Settings → Developer settings → Personal access tokens
git remote set-url origin https://YOUR_USERNAME:YOUR_TOKEN@github.com/mauriciotcec-dev/backend-data.git
git push origin your-branch
```

**Option B: SSH Key**
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add to GitHub: Settings → SSH and GPG keys → New SSH key
# Copy key: cat ~/.ssh/id_ed25519.pub

# Update remote
git remote set-url origin git@github.com:mauriciotcec-dev/backend-data.git
git push origin your-branch
```

### 👥 Solution 3: Request Access
Contact repository owner (mauriciotcec-dev) to add you as a collaborator.

## Quick Diagnostics

```bash
# Check remote URL
git remote -v

# Check Git user config
git config user.name
git config user.email

# Test SSH connection
ssh -T git@github.com
```

## Common Fixes

**Clear credentials:**
```bash
# macOS
git credential-osxkeychain erase

# Windows  
git credential-manager erase https://github.com

# Linux
git credential-cache exit
```

**Enable 2FA token:**
If you have Two-Factor Authentication, you MUST use a Personal Access Token instead of your password.

## 🐳 Docker Hub Authentication Error

**Error:** "Username and password required" in GitHub Actions

**Quick Fix:**
```bash
# 1. Create Docker Hub token at: hub.docker.com → Account Settings → Security → New Access Token
# 2. Add secrets in GitHub: Settings → Secrets and variables → Actions
#    - DOCKER_USERNAME: your Docker Hub username
#    - DOCKER_PASSWORD: your Docker Hub token
```

See [DOCKER_SETUP.md](DOCKER_SETUP.md) for detailed instructions.

For detailed instructions, see [CONTRIBUTING.md](CONTRIBUTING.md)
