# Contributing to backend-data

Thank you for your interest in contributing! This guide will help you get started.

## Resolving GitHub Push Permission Issues

If you encounter the error: **"You don't have permissions to push to 'mauriciotcec-dev/backend-data' on GitHub"**, this means you don't have direct write access to this repository. Here are your options:

### Option 1: Fork and Pull Request (Recommended for External Contributors)

This is the standard workflow for contributing to repositories you don't own:

1. **Fork the repository**
   - Navigate to https://github.com/mauriciotcec-dev/backend-data
   - Click the "Fork" button in the top-right corner
   - GitHub will create a copy of the repository under your account

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/backend-data.git
   cd backend-data
   ```

3. **Add the original repository as upstream**
   ```bash
   git remote add upstream https://github.com/mauriciotcec-dev/backend-data.git
   ```

4. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

5. **Make your changes and commit**
   ```bash
   git add .
   git commit -m "Description of your changes"
   ```

6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request**
   - Go to your fork on GitHub
   - Click "Pull Request" or "Compare & pull request"
   - Ensure the base repository is `mauriciotcec-dev/backend-data` and the base branch is correct
   - Fill in the PR description and submit

8. **Keep your fork updated**
   ```bash
   git fetch upstream
   git checkout main
   git merge upstream/main
   git push origin main
   ```

### Option 2: Request Collaborator Access

If you need direct push access:

1. Contact the repository owner (mauriciotcec-dev)
2. Provide your GitHub username
3. Wait for an invitation to be sent to your email
4. Accept the invitation from GitHub
5. You'll then have push access to the repository

### Option 3: Fix Authentication Issues

If you believe you should have access but are getting this error, your authentication might be misconfigured:

#### Using Personal Access Token (HTTPS)

1. **Generate a Personal Access Token**
   - Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Click "Generate new token (classic)"
   - Select scopes: at minimum check `repo` for full repository access
   - Generate and copy the token (you won't see it again!)

2. **Update your remote URL with the token**
   ```bash
   git remote set-url origin https://YOUR_USERNAME:YOUR_TOKEN@github.com/mauriciotcec-dev/backend-data.git
   ```

3. **Or use Git credential helper**
   ```bash
   # Configure credential helper
   git config --global credential.helper cache
   # Or for permanent storage (less secure)
   git config --global credential.helper store
   
   # Then push, and enter your token when prompted for password
   git push origin your-branch
   ```

#### Using SSH Keys

1. **Generate an SSH key** (if you don't have one)
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   # Press Enter to accept default location
   # Optionally set a passphrase
   ```

2. **Add SSH key to ssh-agent**
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

3. **Add SSH key to GitHub**
   - Copy your public key: `cat ~/.ssh/id_ed25519.pub`
   - Go to GitHub Settings → SSH and GPG keys → New SSH key
   - Paste the key and save

4. **Update your remote URL to SSH**
   ```bash
   git remote set-url origin git@github.com:mauriciotcec-dev/backend-data.git
   ```

5. **Test your connection**
   ```bash
   ssh -T git@github.com
   ```

## Troubleshooting

### Check your current configuration
```bash
# View remote URLs
git remote -v

# View your Git user configuration
git config --list | grep user

# Check which credential helper is active
git config --list | grep credential
```

### Clear stored credentials

**macOS:**
```bash
git credential-osxkeychain erase
host=github.com
protocol=https
# Press Enter twice
```

**Windows:**
```bash
git credential-manager erase https://github.com
```

**Linux:**
```bash
# Remove credential helper
git config --global --unset credential.helper

# Or clear cache
git credential-cache exit
```

### Common Issues

1. **Two-Factor Authentication (2FA)**
   - If you have 2FA enabled, you must use a Personal Access Token instead of your password
   - Regular passwords won't work with HTTPS Git operations

2. **Organization Repositories**
   - Check if you need to enable SSO for your Personal Access Token
   - Go to token settings and click "Enable SSO" for the organization

3. **Wrong Remote URL**
   - Ensure you're pushing to the correct repository
   - Use `git remote -v` to verify

4. **Branch Protection Rules**
   - The repository might have branch protection that prevents direct pushes
   - You may need to create a Pull Request even with write access

## Development Workflow

1. Always create a new branch for your changes
2. Write clear, descriptive commit messages
3. Test your changes before pushing
4. Keep your commits focused and atomic
5. Update documentation as needed
6. Follow the project's coding standards

## Docker Hub Authentication for CI/CD

This repository uses GitHub Actions with `docker/login-action@v3` for Docker Hub authentication.

### Setting Up Docker Secrets

If you encounter the error **"Username and password required"** when running the Docker login workflow:

1. **Create Docker Hub Access Token**
   - Log in to [Docker Hub](https://hub.docker.com)
   - Go to Account Settings → Security → New Access Token
   - Create a token with Read & Write permissions
   - Copy the token immediately (you won't see it again)

2. **Add Secrets to GitHub Repository**
   - Navigate to your repository: Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Add `DOCKER_USERNAME`: Your Docker Hub username
   - Add `DOCKER_PASSWORD`: Your Docker Hub Personal Access Token (not your account password)

3. **Workflow Configuration**
   The workflow file `.github/workflows/docker-login.yml` uses these secrets:
   ```yaml
   - name: Log in to Docker Hub
     uses: docker/login-action@v3
     with:
       username: ${{ secrets.DOCKER_USERNAME }}
       password: ${{ secrets.DOCKER_PASSWORD }}
   ```

### Best Practices

- **Never commit credentials** to the repository
- Use Personal Access Tokens instead of passwords for better security
- Set appropriate token permissions (typically Read & Write for package access)
- Rotate tokens regularly for security
- For organization repositories, ensure SSO is enabled if required

### Troubleshooting Docker Login

**Error: Username and password required**
- Verify `DOCKER_USERNAME` and `DOCKER_PASSWORD` secrets are set in GitHub
- Check that the secrets are accessible to the workflow (repository or organization level)
- Ensure the Docker Hub token is valid and not expired

**Error: unauthorized**
- The token may lack necessary permissions
- Create a new token with Read & Write access

**Error: denied: requested access to the resource is denied**
- Verify the username is correct
- Check repository/image permissions on Docker Hub

## Getting Help

If you continue to experience issues:
- Check GitHub's [authentication documentation](https://docs.github.com/en/authentication)
- Review GitHub's [troubleshooting guide](https://docs.github.com/en/get-started/using-git/troubleshooting-the-push-command)
- Contact the repository maintainer for assistance

Thank you for contributing!
