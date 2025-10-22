# Organization-wide GitHub Configuration

This repository contains organization-wide GitHub configuration files that will be applied to all repositories in the organization.

## What's Included

### Issue Templates

Two issue templates are available across all repositories:

1. **Bug Report** (`ISSUE_TEMPLATE/bug_report.yml`) - For reporting bugs with structured fields
2. **Feature Request** (`ISSUE_TEMPLATE/feature_request.yml`) - For suggesting new features

These templates will automatically appear when users create new issues in any repository.

### Automated Project Management

The workflow `add-to-project.yml` automatically adds newly created issues to your organization project board.

## Setup Instructions

### 1. Enable Issue Templates (No action needed)

Once this repository is pushed to GitHub, the issue templates will automatically be available to all repositories in the organization.

### 2. Configure Automatic Project Addition

To enable automatic addition of issues to your organization project:

#### Step 1: Create a Personal Access Token (PAT)

1. Go to GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. Click "Generate new token"
3. Give it a descriptive name (e.g., "Add to Project Token")
4. Set expiration as needed
5. Under "Repository access", select "All repositories"
6. Under "Permissions", grant:
   - **Organization permissions**:
     - Projects: Read and write
   - **Repository permissions**:
     - Issues: Read
     - Metadata: Read
7. Generate and copy the token

#### Step 2: Add the Token as an Organization Secret

1. Go to your organization Settings → Secrets and variables → Actions
2. Click "New organization secret"
3. Name: `ADD_TO_PROJECT_PAT`
4. Value: Paste your PAT from Step 1
5. Repository access: "All repositories"
6. Click "Add secret"

#### Step 3: Update the Project URL

Edit `.github/workflows/add-to-project.yml` and update the `project-url` with your actual project URL:

```yaml
project-url: https://github.com/orgs/YOUR_ORG_NAME/projects/YOUR_PROJECT_NUMBER
```

To find your project URL:
1. Go to your organization's Projects tab
2. Open the project you want to use
3. Copy the URL from your browser (it should look like: `https://github.com/orgs/closure/projects/1`)

### 3. Deploy

```bash
git add .
git commit -m "Add organization-wide issue templates and automation"
git push
```

## Customization

### Adding More Templates

You can add additional issue templates by creating new `.yml` files in the `ISSUE_TEMPLATE/` directory. Each template should follow the GitHub issue forms syntax.

### Modifying the Config

Edit `ISSUE_TEMPLATE/config.yml` to:
- Enable/disable blank issues
- Add contact links (documentation, support, etc.)

### Extending Automation

The workflow can be extended to:
- Add labels based on issue content
- Assign issues to team members
- Send notifications to Slack/Discord
- Set custom project field values

## How It Works

### Issue Templates

GitHub automatically detects the `.github` repository in an organization and uses its contents as defaults for all repositories that don't have their own templates.

### Automation Workflow

The workflow triggers on every new issue creation and uses the `actions/add-to-project` action to add the issue to the specified project board. This works across all repositories in the organization.

## Troubleshooting

### Templates not showing up?

- Ensure this repository is named `.github` (lowercase)
- Push the changes to the `main` or `master` branch
- Wait a few minutes for GitHub to propagate the changes
- Individual repositories with their own templates will override these

### Workflow not working?

- Verify the `ADD_TO_PROJECT_PAT` secret exists and has correct permissions
- Check that the project URL is correct and the project exists
- Ensure the PAT hasn't expired
- Review the Actions tab in any repository to see workflow run logs

## Additional Resources

- [GitHub Issue Forms Documentation](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms)
- [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
- [GitHub Actions: Add to Project](https://github.com/actions/add-to-project)

