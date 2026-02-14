# GitHub Branch Protection Setup Guide

This guide will walk you through setting up branch protection rules and completing the workflow configuration.

---

## Files Created

The following files have been created in your repository:

✅ [`.github/workflows/ci.yml`](.github/workflows/ci.yml) - CI/CD pipeline
✅ [`vercel.json`](vercel.json) - Vercel deployment configuration
✅ [`CONTRIBUTING.md`](CONTRIBUTING.md) - Developer workflow guide
✅ [`.github/pull_request_template.md`](.github/pull_request_template.md) - PR template

---

## Step 1: Push `develop` Branch to Remote

The `develop` branch has been created locally but needs to be pushed to remote. Due to a permission issue, you'll need to do this manually:

```bash
# Navigate to your repository
cd ABHUDAYA

# Verify you're on the develop branch
git branch

# Push to remote (use your Git credentials)
git push -u origin develop
```

**If you get a 403 permission error:**
- Check your Git credentials: `git config user.name` and `git config user.email`
- Ensure you have push access to the repository
- If using HTTPS, you may need to use a Personal Access Token instead of password
- If using SSH, ensure your SSH key is properly configured: `ssh -T git@github.com`

---

## Step 2: Configure Branch Protection Rules

Go to your GitHub repository and configure branch protection:

### Navigate to Settings:
1. Open `https://github.com/Achintya1800/ABHUDAYA/settings/branches`
2. Click "Add branch protection rule"

### For `main` branch:

1. Click "Add rule" and enter branch name pattern: `main`
2. Configure the following settings:

   **Branch protection settings**
   - ✅ Require a pull request before merging
   - ✅ Require approvals: **1** (increase to 2+ as team grows)
   - ✅ Dismiss stale reviews when new commits are pushed
   - ✅ Require review from CODEOWNERS (optional)
   - ✅ Restrict who can dismiss reviews: **Only users with admin access**

   **Require status checks to pass before merging**
   - ✅ Require branches to be up to date before merging
   - ✅ Add required status checks:
     - `Frontend - Lint`
     - `Frontend - Type Check`
     - `Frontend - Build`
     - `Backend - Lint`
     - `Backend - Type Check`
     - `Backend - Build`

   **Additional protections**
   - ✅ Do not allow bypassing the above settings
   - ✅ Require linear history (optional but recommended)
   - ✅ Restrict who can push to matching branches: **Only users with admin access**

3. Click "Create" or "Save changes"

### For `develop` branch:

Repeat the same process for `develop` branch:
1. Add another branch protection rule
2. Branch name pattern: `develop`
3. Apply the same settings as above

---

## Step 3: Connect Repository to Vercel

### If not already connected:

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "Add New" → "Project"
3. Import your GitHub repository: `Achintya1800/ABHUDAYA`
4. Configure your project:
   - **Framework Preset**: Next.js
   - **Root Directory**: `./frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `.next`

### Environment Variables in Vercel:

Set these up for both **Production** and **Preview** environments:

**Production** (for `main` branch):
```
NEXT_PUBLIC_API_URL=https://abhudaya.vercel.app/api
NODE_ENV=production
```

**Preview** (for `develop` branch):
```
NEXT_PUBLIC_API_URL=https://abhudaya-staging.vercel.app/api
NODE_ENV=development
```

Add your other environment variables:
- Database connection strings
- Google Gemini API keys
- Google OAuth credentials
- Any other API keys

### Deployment Branches:

After connecting, configure which branches deploy:
- Go to Project Settings → Git
- Set **Production Branch**: `main`
- Set **Preview Branches**: `develop` (or `*` for all branches)

---

## Step 4: Test the Workflow

### Test CI/CD Pipeline:

1. Create a test branch:
```bash
git checkout develop
git pull origin develop
git checkout -b feature/test-workflow
```

2. Make a small change (like updating README)
3. Commit and push:
```bash
git add .
git commit -m "test: verify CI/CD workflow"
git push -u origin feature/test-workflow
```

4. Create a PR on GitHub targeting `develop`
5. Verify:
   - CI checks run automatically in Actions tab
   - All checks pass (lint, type-check, build)
   - PR shows all green checks

### Test Branch Protection:

1. Try to push directly to `main`:
```bash
# This should FAIL
git checkout main
git echo "test" >> test.txt
git add .
git commit -m "test direct push"
git push origin main  # Should be rejected
```

2. Confirm you get an error: "Branch protection rules blocked this push"

---

## Step 5: Weekly Release Process

### Merge `develop` → `main`:

```bash
# 1. Update both branches locally
git checkout main
git pull origin main

git checkout develop
git pull origin develop

# 2. Merge develop into main
git checkout main
git merge develop

# 3. Push to trigger production deployment
git push origin main

# 4. Tag the release (optional but recommended)
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

### On GitHub:
1. Go to repository → Settings → Branches
2. The main branch should now show "Protected" badge
3. Verify Vercel deploys production from `main`

---

## Developer Onboarding Checklist

For each developer on your team:

- [ ] Read [CONTRIBUTING.md](CONTRIBUTING.md)
- [ ] Configure local `.env` files (never commit these!)
- [ ] Install dependencies: `cd frontend && npm install` and `cd backend && npm install`
- [ ] Create feature branch from `develop`
- [ ] Follow commit message conventions
- [ ] Create PR using the template
- [ ] Wait for CI checks to pass
- [ ] Request review before merge

---

## Troubleshooting

### CI Checks Failing?

1. **Lint errors**: Run `npm run lint` in frontend/backend to see issues
2. **Type errors**: Run `npx tsc --noEmit` to see type errors
3. **Build failures**: Check build logs in GitHub Actions

### Can't Push to Protected Branch?

This is expected behavior! You must:
1. Create a feature branch
2. Make changes there
3. Create a pull request
4. Get approval and pass CI
5. Merge via PR

### Vercel Not Deploying?

1. Check Vercel Dashboard → Deployments tab
2. Verify branch is connected correctly
3. Check environment variables are set
4. Review build logs in Vercel

---

## Quick Reference URLs

- **GitHub Repository**: https://github.com/Achintya1800/ABHUDAYA
- **Branch Protection Settings**: https://github.com/Achintya1800/ABHUDAYA/settings/branches
- **GitHub Actions**: https://github.com/Achintya1800/ABHUDAYA/actions
- **Vercel Dashboard**: https://vercel.com/dashboard

---

## Next Steps

1. ✅ Push `develop` branch to remote
2. ✅ Configure branch protection rules on GitHub
3. ✅ Connect repository to Vercel
4. ✅ Test the workflow with a sample PR
5. ✅ Share [CONTRIBUTING.md](CONTRIBUTING.md) with your team

Your Git workflow is now ready! 🚀
