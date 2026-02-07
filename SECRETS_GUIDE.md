# How to Use Repository Secret (ACCESS_TOKEN)

This repository uses a GitHub repository secret called `ACCESS_TOKEN` to automatically generate and update GitHub statistics cards.

## What is the SECRET used for?

The `ACCESS_TOKEN` secret is used in the `.github/workflows/grs.yml` workflow file to:
1. Fetch your GitHub statistics
2. Generate an SVG card with your stats
3. Commit and push the generated card back to the repository

## Setup Instructions

### 1. Create a Personal Access Token (PAT)

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token" → "Generate new token (classic)"
3. Give it a descriptive name like "GitHub Stats Generator"
4. Set expiration (recommend: No expiration or 1 year)
5. Select the following scopes:
   - `repo` (Full control of private repositories)
   - `read:user` (Read user profile data)
6. Click "Generate token"
7. **IMPORTANT**: Copy the token immediately (you won't be able to see it again!)

### 2. Add the Token as a Repository Secret

1. Go to your repository on GitHub
2. Navigate to: Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `ACCESS_TOKEN` (must be uppercase to match the workflow)
5. Value: Paste the Personal Access Token you copied in step 1
6. Click "Add secret"

### 3. How the Workflow Uses the Secret

In `.github/workflows/grs.yml`, the secret is referenced as:

```yaml
- name: Generate stats card
  uses: readme-tools/github-readme-stats-action@v1
  with:
    card: stats
    options: username=${{ github.repository_owner }}&show_icons=true
    path: profile/stats.svg
    token: ${{ secrets.ACCESS_TOKEN }}  # ← This is where your secret is used
```

The syntax `${{ secrets.ACCESS_TOKEN }}` tells GitHub Actions to:
- Retrieve the secret value from your repository secrets
- Use it securely (it will be masked in logs)
- Pass it to the action for authentication

### 4. Verify It's Working

1. Go to the "Actions" tab in your repository
2. Click on "Update README cards" workflow
3. Click "Run workflow" to manually trigger it
4. Wait for it to complete
5. Check the `profile/` directory for the generated `stats.svg` file

## Workflow Permissions

The workflow has been configured with:
```yaml
permissions:
  contents: write
```

This allows the workflow to:
- Checkout the repository
- Commit generated files
- Push changes back to the repository

## Troubleshooting

### Secret not found error
- Make sure the secret is named exactly `ACCESS_TOKEN` (uppercase)
- Verify the secret exists in: Repository Settings → Secrets and variables → Actions

### Permission denied when pushing
- Ensure the workflow has `permissions: contents: write`
- Verify your PAT has the `repo` scope

### Stats card not generating
- Check that your PAT is valid and not expired
- Verify the PAT has `read:user` permissions
- Check the workflow logs for specific error messages

## Security Best Practices

✅ **DO:**
- Use repository secrets for tokens (never hardcode them)
- Set token expiration dates
- Use the minimum required permissions
- Regularly rotate tokens

❌ **DON'T:**
- Share your Personal Access Token with anyone
- Commit tokens directly in code
- Use tokens with excessive permissions
- Forget to set the secret in repository settings

## Additional Resources

- [GitHub Secrets Documentation](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [Personal Access Tokens Guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
- [GitHub Actions Permissions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#permissions)
