# Local Code Review Workflow

This memory is for conducting code reviews locally (not in GitHub Pull Request context).

## Steps for Local Code Review

When asked to perform a code review locally, follow these steps:

1. **Update the current feature branch**
   ```bash
   git pull
   ```
   Ensures we have the latest version of the feature branch we're reviewing.

2. **Fetch the latest main branch from origin**
   ```bash
   git fetch origin
   ```
   Gets the latest version of the main branch from the remote repository without switching branches.

3. **Generate diff against the main branch**
   ```bash
   git diff origin/main
   ```
   **Important:** Always use `origin/main` (not just `main`) to ensure we're comparing against the latest version of the main branch from the remote repository, not a potentially outdated local copy.

## When to Use This Workflow

- **Use:** When performing local code reviews requested by the user
- **Don't use:** When the review is triggered by GitHub Actions

## Key Points

- This workflow ensures we're always reviewing against the most up-to-date version of both branches
- Using `origin/main` prevents issues with outdated local main branch references
- The workflow is specifically for local development review scenarios