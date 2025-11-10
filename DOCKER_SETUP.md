# Docker Hub Authentication Setup

## Overview

This repository uses GitHub Actions to authenticate with Docker Hub using the `docker/login-action@v3` action.

## The Error

If you see this error when running GitHub Actions workflows:

```
Error: Username and password required
```

This means the Docker login action cannot find the required credentials.

## Solution: Configure GitHub Secrets

### Step 1: Create Docker Hub Access Token

1. Log in to [Docker Hub](https://hub.docker.com)
2. Click on your username (top right) → **Account Settings**
3. Navigate to **Security** tab
4. Click **New Access Token**
5. Configure the token:
   - **Description**: `GitHub Actions backend-data` (or any descriptive name)
   - **Access permissions**: Select **Read & Write** (or **Read, Write, Delete** if needed)
6. Click **Generate**
7. **Copy the token immediately** - you won't be able to see it again!

### Step 2: Add Secrets to GitHub Repository

1. Navigate to your GitHub repository
2. Go to **Settings** (repository settings, not your account)
3. In the left sidebar, click **Secrets and variables** → **Actions**
4. Click **New repository secret**
5. Add the first secret:
   - **Name**: `DOCKER_USERNAME`
   - **Secret**: Your Docker Hub username (e.g., `johndoe`)
   - Click **Add secret**
6. Add the second secret:
   - **Name**: `DOCKER_PASSWORD`
   - **Secret**: The Personal Access Token you created in Step 1
   - Click **Add secret**

### Step 3: Verify Configuration

The workflow file `.github/workflows/docker-login.yml` should now work correctly. It references the secrets like this:

```yaml
- name: Log in to Docker Hub
  uses: docker/login-action@v3
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
```

## Alternative: Using GitHub Container Registry (GHCR)

If you prefer to use GitHub Container Registry instead of Docker Hub:

```yaml
- name: Log in to GitHub Container Registry
  uses: docker/login-action@v3
  with:
    registry: ghcr.io
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}
```

No additional secrets needed! `GITHUB_TOKEN` is automatically provided by GitHub Actions.

## Security Best Practices

### ✅ DO

- Use Personal Access Tokens instead of account passwords
- Set minimal required permissions on tokens
- Use descriptive names for tokens to track their usage
- Rotate tokens periodically (every 90-180 days)
- Revoke tokens immediately if compromised
- Use organization-level secrets for shared repositories

### ❌ DON'T

- Never commit Docker credentials to the repository
- Don't use your Docker Hub account password directly
- Don't share tokens between multiple projects without good reason
- Don't give tokens more permissions than necessary
- Don't log or print secret values in workflows

## Troubleshooting

### Problem: "Username and password required"

**Cause**: Secrets are not configured or not accessible.

**Solutions**:
1. Verify both `DOCKER_USERNAME` and `DOCKER_PASSWORD` secrets exist
2. Check secret names match exactly (case-sensitive)
3. Ensure secrets are at repository level or organization level with proper access
4. For forked repositories, secrets need to be added to your fork

### Problem: "unauthorized: incorrect username or password"

**Cause**: Invalid credentials.

**Solutions**:
1. Verify the username is correct (it's case-sensitive)
2. If using a token, ensure it hasn't expired
3. Create a new Personal Access Token and update the `DOCKER_PASSWORD` secret
4. Check if 2FA is enabled and you're using a token (not password)

### Problem: "denied: requested access to the resource is denied"

**Cause**: Insufficient permissions.

**Solutions**:
1. Create a new token with Read & Write permissions
2. Verify you have access to the Docker Hub repository you're trying to push to
3. Check if the repository exists and you have the correct permissions

### Problem: Secrets not working in forked repository

**Cause**: By default, secrets from the original repository are not accessible to forks.

**Solutions**:
1. Add the secrets to your forked repository
2. Or, for trusted contributors, the repository owner can enable "Run workflows from fork pull requests" with access to secrets

## Testing Your Setup

After configuring secrets, test the workflow:

1. Make a small change to any file (or use workflow_dispatch)
2. Commit and push to trigger the workflow
3. Go to **Actions** tab in your repository
4. Watch the "Docker Login" workflow run
5. Verify the "Log in to Docker Hub" step succeeds

## Workflow Triggers

The Docker login workflow runs on:
- **Push** to `main` or `master` branches
- **Pull requests** to `main` or `master` branches
- **Manual trigger** via workflow_dispatch (Actions tab → Docker Login → Run workflow)

## Additional Resources

- [Docker login action documentation](https://github.com/docker/login-action)
- [GitHub Actions secrets documentation](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [Docker Hub access tokens](https://docs.docker.com/docker-hub/access-tokens/)
- [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

## Need Help?

If you continue experiencing issues:
1. Check the workflow logs in the Actions tab for detailed error messages
2. Verify your Docker Hub credentials work by logging in manually: `docker login`
3. Review this documentation and ensure all steps were followed
4. Contact the repository maintainer for assistance
