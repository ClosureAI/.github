# Code Review and Merge Process

All significant code changes must be reviewed and approved by a peer before being merged into any production branch. This document outlines the required process.

## Branch Naming Convention

Create branches using one of the following prefixes, followed by `/` and a kebab-case description:

| Prefix | Use Case | Example |
|--------|----------|---------|
| `feature/` | New functionality | `feature/user-authentication` |
| `bugfix/` | Bug fixes | `bugfix/button-wont-click` |
| `task/` | General tasks, refactoring | `task/update-dependencies` |
| `devops/` | Infrastructure, CI/CD, deployment | `devops/add-staging-pipeline` |

## Development Workflow

1. **Create your branch** from the appropriate base branch using the naming convention above.

2. **Push code frequently** while work is in progress. Regular pushes ensure your work is backed up and visible to the team.

3. **Create a Pull Request (PR)** early and keep it in **draft status** until the code is ready for review. This allows for early visibility and feedback if needed.

4. **Request a review** from a peer when the code is complete and ready to merge. Remove the draft status and assign the reviewer.

5. **Address feedback** if changes are requested. Push additional commits to the same branch until approval is granted.

## Merge Path to Production

Code must progress through environments in order:

```
feature branch → dev → qa → production
```

| Step | Action | Requirements |
|------|--------|--------------|
| 1 | Merge to `dev` | PR approved by peer |
| 2 | Merge to `qa` | Code validated in dev environment |
| 3 | Merge to `production` | Code validated in QA environment |

**Important:** Code cannot skip environments. All changes must flow through `dev` → `qa` → `production` in sequence.

## Summary

- Branch using: `feature/`, `bugfix/`, `task/`, or `devops/` prefix
- Push often, use draft PRs until ready
- All production merges require approval from CTO
- Follow the merge path: `dev` → `qa` → `production`
