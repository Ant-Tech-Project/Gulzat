Explain the purpose of tags and how to create and manage them in Git: 
Useful links: 
1. https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag
2. https://git-scm.com/book/en/v2/Git-Basics-Tagging
Git tags are a powerful feature for managing versions and releases of your software projects. Tags provide a stable reference point for specific commits in your Git history, making it easy to identify and work with specific codebase versions.

In Git, tags are used to mark specific points in Git history as being important. They are typically used to capture a point in time that is used for a marked version release (e.g., a software release). Tags provide a way to give human-readable names to specific commit hashes and make it easier to reference and manage specific versions of your code.

Here's how you can create and manage tags in Git:
### Creating Tags:
#### 1. Lightweight Tags:
A lightweight tag is simply a reference to a specific commit. It's like a branch that doesn't change.
# Create a lightweight tag
git tag v1.0.0
# Create a lightweight tag for a specific commit
git tag v1.1.0 <commit-hash>

#### 2. Annotated Tags:
An annotated tag is stored as a full Git object, including a tagger name, email, date, and a tagging message.
# Create an annotated tag
git tag -a v2.0.0 -m "Release version 2.0.0"
# Create an annotated tag for a specific commit
git tag -a v2.1.0 -m "Release version 2.1.0" <commit-hash>

### Viewing Tags:
# List all tags
git tag

# Show details for a specific tag
git show v1.0.0

### Pushing Tags:
By default, when you push changes, tags are not pushed to the remote repository. You need to explicitly push tags.
# Push a specific tag
git push origin v1.0.0
# Push all tags
git push --tags

### Deleting Tags:
# Delete a local tag
git tag -d v1.0.0
# Delete a remote tag
git push --delete origin v1.0.0

### Checking Out Tags:
You can check out a specific tag to create a detached HEAD state (not on any branch).
# Checkout a specific tag
git checkout v1.0.0

Tags are useful for marking releases, creating stable points in your project's history, and providing a clear reference to important versions. They also play a crucial role in managing versioning and release workflows.

2. Concept of rebase and its usage
Git Rebase:

# Start a new branch (if not on one already)
git checkout -b feature-branch

# Make changes and commit
git add .
git commit -m "Committing changes on feature-branch"

# Switch to the branch to be rebased onto
git checkout main

# Start the rebase
git pull origin main --rebase
b. Resolve Conflicts (Same as Git Merge):
# Conflicts will be marked in files
# Open conflicted file(s) in a text editor
# Resolve conflict markers (<<<<<<<, =======, >>>>>>>)
# Manually choose changes and remove conflict markers
# Add the resolved files
git add .
# Complete the rebase
git rebase --continue 

3.Compare rebase with merging 
links: 
1. https://www.simplilearn.com/git-rebase-vs-merge-article#:~:text=considerably%20more%20profound.-,What's%20the%20Difference%20Between%20Merge%20and%20Rebase%3F,one%20branch%20onto%20another%20branch.
2. https://www.atlassian.com/git/tutorials/merging-vs-rebasing

The main difference between git merge and git rebase is that git merge is a way of combining changes from one branch (source branch) into another branch (target branch) where as git rebase is a way of moving the changes from one branch onto another branch.
1. Git Merge lets you merge different Git branches.
   Git Rebase allows you to integrate the changes from one branch into another.
2. Git Merge logs show you the complete history of commit merging.
   Git Rebase logs are linear. As the commits are rebased, the history is altered to reflect this.
3. All the commits on a feature branch are combined into a single commit on the master branch.
   All commits are rebased, and the same number of commits are added to the master branch.
4. Merge is best used when the target branch is supposed to be shared.
   Rebase is best used when the target branch is private.
5. Merge preserves history.
   Rebase rewrites history.
