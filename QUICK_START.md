# Quick Start: Using ACCESS_TOKEN Secret

## TL;DR - 3 Steps to Get Started

### Step 1: Create Personal Access Token
1. Go to: https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Select scopes: `repo` + `read:user`
4. Copy the generated token

### Step 2: Add to Repository Secrets
1. Go to: https://github.com/maxven23/maxven23/settings/secrets/actions
2. Click "New repository secret"
3. Name: `ACCESS_TOKEN`
4. Value: [paste your token]
5. Click "Add secret"

### Step 3: Run the Workflow
1. Go to: https://github.com/maxven23/maxven23/actions
2. Click "Update README cards"
3. Click "Run workflow"
4. Wait for completion
5. Check `profile/stats.svg` was created

## What Happens Next?

The workflow will:
- ✅ Run daily at 3 AM UTC (scheduled)
- ✅ Can be triggered manually anytime
- ✅ Generate GitHub stats SVG card
- ✅ Commit to `profile/stats.svg`
- ✅ Push changes automatically

## Need More Details?

See [SECRETS_GUIDE.md](./SECRETS_GUIDE.md) for comprehensive documentation.

## Current Workflow Configuration

```yaml
token: ${{ secrets.ACCESS_TOKEN }}  # Line 22 in .github/workflows/grs.yml
```

The workflow is configured to use `ACCESS_TOKEN` (uppercase). Make sure your secret matches this name exactly!
