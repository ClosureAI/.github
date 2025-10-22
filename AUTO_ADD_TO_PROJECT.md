# Automatically Add Issues to Organization Project

There are **two ways** to automatically add issues to your organization project:

## ✅ Option 1: Built-in GitHub Project Automation (RECOMMENDED)

This is the easiest way - no workflows needed!

### Steps:

1. **Go to your project**: https://github.com/orgs/ClosureAI/projects/2
2. Click the **⋯** (three dots) in the top right
3. Select **"Workflows"**
4. Find the **"Auto-add to project"** workflow
5. Configure it:
   - **Trigger**: "Issues opened"
   - **Filters**: Choose which repos (select "All repositories in ClosureAI")
   - **Action**: "Add to project"
6. **Save**

Now every new issue in any repository will automatically be added to your project! 🎉

---

## Option 2: GitHub Actions Workflow (More Control)

Use this if you need custom logic or specific conditions.

### Setup:

1. **Create a Personal Access Token (PAT)**:
   - Go to: https://github.com/settings/tokens
   - Click "Generate new token (classic)"
   - Name: `Organization Project Automation`
   - Scopes needed:
     - ✅ `repo` (Full control of repositories)
     - ✅ `project` (Full control of projects)
   - Generate and copy the token

2. **Add as Organization Secret**:
   - Go to: https://github.com/organizations/ClosureAI/settings/secrets/actions
   - Click "New organization secret"
   - Name: `ORG_PROJECT_TOKEN`
   - Value: Paste your token
   - Repository access: "All repositories"

3. **Add workflow to each repository**:
   
   Create `.github/workflows/add-to-project.yml` in each repo:

   ```yaml
   name: Add Issue to Project

   on:
     issues:
       types: [opened]

   jobs:
     add-to-project:
       runs-on: ubuntu-latest
       steps:
         - name: Add issue to project
           uses: actions/add-to-project@v1.0.2
           with:
             project-url: https://github.com/orgs/ClosureAI/projects/2
             github-token: ${{ secrets.ORG_PROJECT_TOKEN }}
   ```

### Quick Add Script:

To add the workflow to multiple repositories at once, run:

```bash
#!/bin/bash
# Add this workflow to all your repos

REPOS=("advocate-app" "repo2" "repo3")  # Add your repo names

for repo in "${REPOS[@]}"; do
  echo "Adding workflow to $repo..."
  git clone git@github.com:ClosureAI/$repo.git /tmp/$repo
  cd /tmp/$repo
  mkdir -p .github/workflows
  curl -o .github/workflows/add-to-project.yml \
    https://raw.githubusercontent.com/ClosureAI/.github/main/workflow-templates/add-to-project.yml
  git add .github/workflows/add-to-project.yml
  git commit -m "Add auto-add to project workflow"
  git push
  cd -
done
```

---

## 🎯 Which Should You Choose?

| Feature | Built-in Automation | GitHub Actions |
|---------|-------------------|----------------|
| Setup time | 2 minutes | 10-15 minutes |
| Maintenance | None | Need to manage PAT |
| Customization | Limited | Full control |
| Filtering | Basic | Advanced (labels, titles, etc.) |
| **Recommendation** | **Use this first** | Use if you need custom logic |

---

## 🔧 Advanced Customization

If using GitHub Actions, you can add conditions:

### Only add bugs:
```yaml
jobs:
  add-to-project:
    runs-on: ubuntu-latest
    if: contains(github.event.issue.labels.*.name, 'bug')
    steps:
      # ... rest of workflow
```

### Only add from specific repos:
```yaml
jobs:
  add-to-project:
    runs-on: ubuntu-latest
    if: github.repository == 'ClosureAI/advocate-app'
    steps:
      # ... rest of workflow
```

### Set custom field values:
```yaml
- name: Add to project and set status
  uses: actions/add-to-project@v1.0.2
  with:
    project-url: https://github.com/orgs/ClosureAI/projects/2
    github-token: ${{ secrets.ORG_PROJECT_TOKEN }}
    
- name: Set fields
  uses: titoportas/update-project-fields@v0.1.0
  with:
    project-url: https://github.com/orgs/ClosureAI/projects/2
    github-token: ${{ secrets.ORG_PROJECT_TOKEN }}
    item-id: ${{ steps.add-to-project.outputs.itemId }}
    field-keys: Status,Priority
    field-values: Todo,Medium
```

---

## 🐛 Troubleshooting

### Workflow not triggering?
- Check that Actions are enabled in repository settings
- Verify the workflow file is on the default branch (main/master)
- Check workflow runs in the "Actions" tab

### "Resource not accessible" error?
- Token needs both `repo` and `project` scopes
- For classic PAT: Make sure `project` is checked under "admin:org"
- For fine-grained PAT: Need "Projects: Read and write" organization permission

### Token expired?
- Classic tokens can be set to never expire (for automation)
- Or set a reminder to rotate before expiration
- Update the organization secret with the new token

---

## 📚 Additional Resources

- [GitHub Actions: Add to Project](https://github.com/actions/add-to-project)
- [GitHub Projects Automation](https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-built-in-automations)
- [Managing Project Workflows](https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project)

