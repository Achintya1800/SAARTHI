# Contributing to ABHUDAYA

Thank you for your interest in contributing to ABHUDAYA! This guide will help you understand our development workflow and coding standards.

---

## Git Workflow

We follow a structured Git workflow to maintain code quality and ensure smooth deployments:

### Branch Structure

- **`main`** - Production branch (protected, deploys to production)
- **`develop`** - Staging branch (protected, deploys to staging/preview)
- **Feature branches** - Developer branches for individual features/bugfixes

### Branch Naming Convention

Please use one of these prefixes for your branches:

- `feature/` - New features (e.g., `feature/user-authentication`)
- `bugfix/` - Bug fixes (e.g., `bugfix/login-error`)
- `hotfix/` - Urgent production fixes (e.g., `hotfix/security-patch`)
- `refactor/` - Code refactoring (e.g., `refactor/api-structure`)
- `docs/` - Documentation changes (e.g., `docs/api-readme`)

You can also use your name followed by feature description:
- `achintya/feature-name` or `developer/feature-name`

---

## Development Workflow

### Step 1: Start from `develop` branch

Always create your feature branch from the latest `develop` branch:

```bash
# Make sure you're on develop
git checkout develop

# Pull latest changes
git pull origin develop

# Create your feature branch
git checkout -b feature/your-feature-name
```

### Step 2: Make your changes

Develop your feature with proper commits:

```bash
# Stage your changes
git add .

# Commit with a descriptive message
git commit -m "feat: add user authentication with Google OAuth"
```

**Commit Message Format**:
- `feat:` - New feature
- `fix:` - Bug fix
- `refactor:` - Code refactoring
- `docs:` - Documentation changes
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

### Step 3: Push to your branch

```bash
git push -u origin feature/your-feature-name
```

### Step 4: Create Pull Request

1. Go to GitHub repository
2. Click "Compare & pull request" for your branch
3. Target: `develop` branch (NOT main)
4. Fill in the PR template
5. Request review from a team member
6. Wait for CI checks to pass

### Step 5: After Merge

Once your PR is approved and merged:

```bash
# Switch back to develop
git checkout develop

# Pull latest changes
git pull origin develop

# Delete your local feature branch
git branch -d feature/your-feature-name
```

---

## Weekly Release Process (Admins Only)

Once per week, we merge `develop` into `main` for production release:

### Step 1: Prepare for Release

```bash
# Ensure main is up to date
git checkout main
git pull origin main

# Switch to develop
git checkout develop
git pull origin develop
```

### Step 2: Merge to Main

```bash
# Switch to main
git checkout main

# Merge develop into main
git merge develop

# Push to trigger production deployment
git push origin main
```

### Step 3: Tag Release (Optional)

```bash
# Create a version tag
git tag -a v1.0.0 -m "Release v1.0.0: Feature summary here"

# Push the tag
git push origin v1.0.0
```

---

## CI/CD Pipeline

All pull requests to `main` or `develop` branches must pass CI checks:

### Automated Checks

- **Lint** - ESLint checks for code quality
- **Type Check** - TypeScript type verification
- **Build** - Verify both frontend and backend build successfully

### What to Do If CI Fails

1. Check the GitHub Actions tab for detailed error logs
2. Fix the issues locally
3. Commit and push fixes to your branch
4. CI will automatically re-run

---

## Code Standards

### Frontend (Next.js)

- Use TypeScript for type safety
- Follow existing component structure
- Use Tailwind CSS for styling
- Write responsive, accessible UI

### Backend (Express.js)

- Use ES modules (`import`/`export`)
- Follow RESTful API conventions
- Handle errors appropriately
- Add proper validation for API inputs

---

## Environment Variables

### Local Development

Create `.env.local` files in both `frontend/` and `backend/`:

```bash
# frontend/.env.local
NEXT_PUBLIC_API_URL=http://localhost:5000/api
NODE_ENV=development

# backend/.env.local
PORT=5000
MONGODB_URI=mongodb://localhost:27017/abhudaya
# Add other API keys here
```

### Never Commit `.env` Files

Environment files are already in `.gitignore`. Never commit secrets!

---

## Testing Deployments

### Staging (develop branch)

After merging to `develop`, check the staging deployment:
- Wait for Vercel deployment to complete
- Test your changes on the staging URL
- Verify everything works as expected

### Production (main branch)

After weekly release:
- Production deploys automatically when pushing to `main`
- Test critical functionality on production URL
- Monitor for any errors

---

## Need Help?

If you have questions:

1. Check existing documentation in the repo
2. Ask in team discussions
3. Create an issue if you find a bug
4. Start a discussion for feature proposals

---

## Quick Reference

```bash
# Create feature branch
git checkout develop && git pull && git checkout -b feature/name

# Commit changes
git add . && git commit -m "feat: description"

# Push and create PR
git push -u origin feature/name

# After merge
git checkout develop && git pull && git branch -d feature/name

# Weekly release (admins)
git checkout main && git pull && git merge develop && git push origin main
```

---

Happy coding! 🚀
