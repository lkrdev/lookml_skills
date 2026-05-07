---
name: lookml-project
description: Use this skill when you need to create a new LookML project. This skill handles creating the project in Looker and configuring it for either bare mode or connecting it to a remote Git repository (like GitHub), including automating GitHub setup if the `gh` CLI is available.
---

# Instructions

Use this skill to guide the user through creating and configuring a new Looker project. You must support both **bare mode** (local to Looker) and **remote Git mode** (e.g., GitHub).

## Step 1: Ask the User for Preferences
Before taking any action, you **must** ask the user:
1. What should the new project be named?
2. Do they want a **Bare** project (no remote Git, local to Looker) or a **Git-connected** project (connected to GitHub or another provider)?

---

## Step 2: Create the Project
1. **Ensure Development Mode is Enabled**: Looker requires Development Mode to create or modify projects. You must toggle the IDE session into Development Mode by invoking the `dev_mode` tool with `devMode: true` before proceeding.
2. **Create the Project**: Once in Development Mode, invoke the `create_project` tool to create the project in Looker.

```json
// Example Tool Call
{
  "name": "create_project",
  "arguments": {
    "name": "my_new_project"
  }
}
```

---

## Step 3: Configure Git (Based on User Preference)

### Path A: Bare Mode
If the user chose **Bare**:
1. Inform the user that you are configuring the project to use Looker's local bare Git repository.
2. Invoke `update_project` to set `git_service_name` to `"bare"`.

```json
{
  "name": "update_project",
  "arguments": {
    "project_id": "my_new_project",
    "git_service_name": "bare"
  }
}
```
3. Inform the user that the bare project is ready!

---

### Path B: Git-Connected Mode (GitHub Automation)
If the user chose **Git-connected**:

1. **Generate Deploy Key**: Invoke `create_git_deploy_key` to generate a public/private SSH key pair in Looker for this project. This tool will return the public deploy key.
   ```json
   {
     "name": "create_git_deploy_key",
     "arguments": {
       "project_id": "my_new_project"
     }
   }
   ```

2. **Check for `gh` CLI**: Check if the `gh` CLI is installed and authenticated on the host system by running:
   `gh auth status`
   
3. **If `gh` is Available & Authenticated (Automated Setup)**:
   - Ask the user for confirmation to automate the GitHub repository creation and deploy key setup.
   - Present options:
     - **Repository Name**: Default to the project name (e.g., `my_new_project`).
     - **Visibility**: Private (Recommended) or Public.
     - **Owner**: Personal account or an organization (if they specify one).
   - Once confirmed, execute the following commands:
     - **Create Repository**:
       `gh repo create <owner>/<repo_name> --private --confirm` (or `--public`)
     - **Add Deploy Key with Write Access** (Crucial for Looker to push changes):
       `echo "<public_deploy_key>" | gh repo deploy-key add - -w -t "Looker Deploy Key - my_new_project"`
     - **Get SSH URL**: The SSH URL will typically be `git@github.com:<owner>/<repo_name>.git`.

4. **If `gh` is NOT Available (Manual Setup)**:
   - Present the generated public deploy key to the user.
   - Instruct the user to:
     1. Create a new repository on their Git provider (e.g., GitHub, GitLab).
     2. Go to Repository Settings -> Deploy Keys.
     3. Add the public deploy key and **ensure "Allow write access" (or write permission) is checked**.
     4. Copy the SSH URL of the repository (e.g., `git@github.com:owner/repo.git`).
   - Ask the user to provide the SSH URL once they have completed these steps.

5. **Update Project Configuration**: Once the remote repository is ready and the deploy key is added, invoke `update_project` to connect Looker to the remote repository.

   > [!IMPORTANT]
   > Do **NOT** specify `git_production_branch_name` (e.g., `"main"`) in this initial `update_project` call. 
   > If Looker has not yet fetched the remote repository, setting this parameter will cause a `422 Validation Failed` error ("Git Production Branch Name is not a valid branch"). 
   > Instead, omit this parameter and let Looker auto-detect the default branch from the remote. You can update the branch later if needed.

   ```json
   {
     "name": "update_project",
     "arguments": {
       "project_id": "my_new_project",
       "git_remote_url": "git@github.com:owner/repo.git",
       "git_service_name": "github" // or "gitlab", etc.
     }
   }
   ```

6. **Synchronize Local Workspace with Looker Dev Branch**:
   Since Looker's Development Mode forces the session into a user-specific developer branch (e.g., `dev-<username>-<hash>`), you must check out and track this same branch locally to avoid branch disconnects.
   1. Retrieve the active Looker branch name using the `get_git_branch` tool.
   2. In the local workspace terminal, check out this branch (create it locally since it won't exist yet in your local git clone):
      `git checkout -b <looker_branch_name>`
   3. Push and track this branch on the remote repository:
      `git push -u origin <looker_branch_name>`
   
   This ensures that any commits made locally can be pushed to GitHub and pulled into Looker, and vice versa, on the exact same branch.

7. Inform the user that the Git-connected project is ready!

---

# Examples

## Bare Mode Workflow Example
1. **User**: "Create a new project named marketing_analytics, bare mode."
2. **Agent**: Calls `create_project(name: "marketing_analytics")`.
3. **Agent**: Calls `update_project(project_id: "marketing_analytics", git_service_name: "bare")`.
4. **Agent**: "Successfully created bare Looker project 'marketing_analytics'!"

## Git Mode Workflow (Automated with `gh`) Example
1. **User**: "Create a new project named finance_reports connected to GitHub."
2. **Agent**: Calls `create_project(name: "finance_reports")`.
3. **Agent**: Calls `create_git_deploy_key(project_id: "finance_reports")` -> returns `"ssh-rsa AAAAB3..."`.
4. **Agent**: Runs `gh auth status` -> Authenticated.
5. **Agent**: "I can automate the GitHub repository setup for you. I recommend creating a **private** repository named `finance_reports` on your personal account. Would you like me to proceed?"
6. **User**: "Yes, go ahead."
7. **Agent**: Runs `gh repo create finance_reports --private -y`.
8. **Agent**: Runs `echo "ssh-rsa AAAAB3..." | gh repo deploy-key add - -w -t "Looker Deploy Key - finance_reports"`.
9. **Agent**: Calls `update_project(project_id: "finance_reports", git_remote_url: "git@github.com:awilson/finance_reports.git", git_service_name: "github")`.
10. **Agent**: "Successfully created 'finance_reports' and connected it to private GitHub repository `awilson/finance_reports`!"
