What is Git Hooks and how to use it?
Git Hooks are scripts that are executed before or after different Git events. For instance: commit, push, and receive. They are a built-in solution (no need to download any third-party addon) and they execute locally on your machine. The variety of scenarios we can apply them on is of considerable size.
For instance, depending on your flow you might want to create a release tag right after you have merged your develop branch into your master branch. This is easily achievable with Git Hooks:

#!/bin/sh
name_of_branch=$(git branch | grep "*" | sed "s/\* //")
if [[ $name_of_branch == "master" ]]; then
	read -p "Branch successfully merged on master. Please, specify tag version and click enter: " version
	echo Creating tag with version "$version"
	git tag -a v$version -m "$version" 
 fi
password = 1111111 
This could in fact be further automated (instead of typing the tag on the console you could read it from your Gradle file, or from an environmental variable where you store it). 

Certainly! Let's dive into Git hooks as the last topic for Git.

## **Git Hooks**

Git hooks are scripts that Git executes before or after events such as commit, push, and receive. They allow you to customize and automate parts of the Git workflow. Git supports both client-side and server-side hooks.

### **Types of Git Hooks**

1. **Pre-commit Hook:**
   - **Purpose:** Executes just before a commit is made. It can be used to check the committed changes, run tests, or enforce coding standards.
   - **Location:** `.git/hooks/pre-commit`

2. **Pre-receive Hook:**
   - **Purpose:** Runs on the server before receiving pushed commits. It can be used to enforce project-specific policies or checks.
   - **Location:** On the server-side in the `hooks` directory of the bare repository.

3. **Post-receive Hook:**
   - **Purpose:** Executes on the server after all refs have been updated. Commonly used for triggering deployment scripts or notifications.
   - **Location:** On the server-side in the `hooks` directory of the bare repository.

4. **Prepare-commit-msg Hook:**
   - **Purpose:** Runs before the commit message editor is fired up. It is used to manipulate the default commit message.
   - **Location:** `.git/hooks/prepare-commit-msg`

5. **Post-commit Hook:**
   - **Purpose:** Executes on the client after a commit is made. Useful for triggering notifications or additional actions.
   - **Location:** `.git/hooks/post-commit`

### **Using Git Hooks**

1. **Activation:**
   - Git hooks need to be executable to run. You can activate a hook by renaming the sample file (e.g., changing `pre-commit.sample` to `pre-commit`) and making it executable.

     ```bash
     chmod +x .git/hooks/pre-commit
     ```

2. **Customization:**
   - Edit the hook script to customize its behavior. For example, in a pre-commit hook, you might run linting tools or tests.

3. **Access to Git Environment:**
   - Hooks have access to the Git environment, allowing them to gather information about the changes being made or to interact with the repository.

4. **Sample Use Case - Pre-commit Hook:**
   - Suppose you want to run linting on your code before allowing a commit. You could create a pre-commit hook that runs a linter (e.g., ESLint) on your staged files.

     ```bash
     #!/bin/bash
     # .git/hooks/pre-commit

     # Run ESLint on staged files
     git diff --cached --name-only --diff-filter=ACM | grep '\.js$' | xargs eslint

     # If ESLint fails, prevent the commit
     if [ $? -ne 0 ]; then
         echo "ESLint failed. Commit aborted."
         exit 1
     fi
     ```

### **Benefits of Git Hooks**

1. **Enforce Policies:**
   - Git hooks help enforce project-specific policies and standards, ensuring consistency in code quality.

2. **Automation:**
   - Automate repetitive tasks such as running tests, formatting code, or triggering deployments.

3. **Custom Workflows:**
   - Customize your Git workflow based on your team's needs and project requirements.

4. **Integrations:**
   - Integrate with external tools or services to enhance the development process.

Git hooks provide a powerful mechanism to extend and automate Git's functionality, making them a valuable tool for teams looking to enforce best practices and streamline their development workflows.

Git hooks are primarily executed as scripts on the local machine or on the server, and they are triggered by specific Git events (such as pre-commit, post-commit, pre-push, etc.). However, using Git hooks to send messages to reviewers after sending a pull request (PR) isn't a common use case directly addressed by Git hooks. This is because Git hooks are more focused on local and server-side actions related to version control.

For tasks like notifying reviewers after a pull request, you might want to explore the features provided by the platform hosting your repository (e.g., GitHub, GitLab, Bitbucket). Many of these platforms offer integrations, webhooks, or automation features that allow you to perform actions based on events like PR creation, merge, or comment.

Here's a general outline of how you might achieve this using a hosting platform:

1. **Use Platform-Specific Features:**
   - Platforms like GitHub provide features such as pull request webhooks or actions/workflows that can be triggered on specific events.

2. **Webhooks:**
   - Configure a webhook to be triggered on events like opening a pull request or pushing changes. The webhook can call an external service or script.

3. **External Service or Script:**
   - Create an external service or script that sends a notification to reviewers. This service could be a custom server, a third-party messaging service, or an API integration.

4. **Platform API:**
   - Utilize the platform's API to gather information about the pull request, reviewers, and any other relevant details.

5. **Send Notifications:**
   - Use the gathered information to send notifications to the reviewers. This could be done through email, messaging platforms, or other communication channels.

Here's a simplified example using GitHub Actions (GitHub's CI/CD and automation feature) as a way to send a message to reviewers:

```yaml
name: Notify Reviewers

on:
  pull_request:
    types:
      - opened

jobs:
  notify-reviewers:
    runs-on: ubuntu-latest

    steps:
    - name: Send Notification
      run: |
        # Add your script or command to send notifications
        echo "Hey, a new pull request is open!" 
        echo "Reviewers: @reviewer1 @reviewer2"
```

Remember to adapt this to your specific hosting platform and the tools or services you use for notifications.

Here is a good link for this: https://www.geeksforgeeks.org/git-hooks/