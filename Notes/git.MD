# Complete Git and GitHub Tutorial

## Table of Contents
1. [Introduction to Version Control](#introduction-to-version-control)
2. [Installing Git](#installing-git)
3. [Git Basics](#git-basics)
4. [Working with Repositories](#working-with-repositories)
5. [Branching and Merging](#branching-and-merging)
6. [Remote Repositories](#remote-repositories)
7. [GitHub Fundamentals](#github-fundamentals)
8. [Collaboration Workflows](#collaboration-workflows)
9. [Advanced Git Features](#advanced-git-features)
10. [Best Practices](#best-practices)
11. [Troubleshooting](#troubleshooting)

---

## Introduction to Version Control

Version control is a system that records changes to files over time, allowing you to recall specific versions later. Git is a distributed version control system that tracks changes in any set of files.

### Why Use Version Control?
- **Track Changes**: See what changed, when, and who made the changes
- **Collaboration**: Multiple people can work on the same project simultaneously
- **Backup**: Your code history is preserved across multiple locations
- **Branching**: Work on different features in isolation
- **Rollback**: Easily revert to previous versions when bugs are introduced

### Git vs GitHub
- **Git**: The version control system (software)
- **GitHub**: A cloud-based hosting service for Git repositories (platform)

---

## Installing Git

### Windows
1. Download from [git-scm.com](https://git-scm.com)
2. Run the installer with default settings
3. Verify installation: `git --version`

### macOS
```bash
# Using Homebrew (recommended)
brew install git

# Or download from git-scm.com
```

### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install git
```

### Initial Configuration
```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor (optional)
git config --global core.editor "code --wait"  # VS Code
```

---

## Git Basics

### Key Concepts

**Repository (Repo)**: A directory containing your project files and Git's tracking information

**Working Directory**: Your current project folder where you edit files

**Staging Area**: A holding area for changes you want to include in your next commit

**Commit**: A snapshot of your project at a specific point in time

**Branch**: An independent line of development

### The Three States of Git
1. **Modified**: You've changed files but haven't committed them
2. **Staged**: You've marked modified files to go into your next commit
3. **Committed**: Data is safely stored in your local database

### Basic Commands

#### Initialize a Repository
```bash
# Create a new repository
git init

# Or clone an existing repository
git clone https://github.com/username/repository.git
```

#### Check Status
```bash
# See the status of files in your working directory
git status

# Short status format
git status -s
```

#### Adding Files
```bash
# Add specific file
git add filename.txt

# Add all files in current directory
git add .

# Add all files with specific extension
git add *.js

# Add all files
git add -A
```

#### Making Commits
```bash
# Commit staged changes
git commit -m "Your commit message"

# Add and commit in one step (tracked files only)
git commit -am "Your commit message"

# Commit with detailed message
git commit  # Opens editor for longer message
```

#### Viewing History
```bash
# View commit history
git log

# Compact view
git log --oneline

# View last N commits
git log -n 5

# View changes in each commit
git log -p

# Graphical representation
git log --graph --oneline --all
```

#### Viewing Changes
```bash
# See changes in working directory
git diff

# See changes in staging area
git diff --staged

# Compare specific commits
git diff commit1 commit2
```

---

## Working with Repositories

### File Lifecycle in Git

```
Untracked → Staged → Committed → Modified → Staged → Committed
     ↓         ↑         ↓         ↑         ↑
   (add)   (add)    (edit)    (add)    (add)
```

### Ignoring Files

Create a `.gitignore` file to specify which files Git should ignore:

```
# Dependencies
node_modules/
*.log

# Build outputs
dist/
build/

# Environment files
.env
.env.local

# OS generated files
.DS_Store
Thumbs.db

# IDE files
.vscode/
.idea/

# Language specific
*.pyc
__pycache__/
```

### Removing Files
```bash
# Remove file from Git and working directory
git rm filename.txt

# Remove file from Git but keep in working directory
git rm --cached filename.txt

# Remove directory
git rm -r directory/
```

### Moving/Renaming Files
```bash
# Rename file
git mv oldname.txt newname.txt

# Move file
git mv file.txt directory/
```

---

## Branching and Merging

Branches allow you to work on different features simultaneously without affecting the main codebase.

### Branch Commands
```bash
# List all branches
git branch

# Create new branch
git branch feature-name

# Switch to branch
git checkout feature-name

# Create and switch to new branch
git checkout -b feature-name

# Modern way to switch branches (Git 2.23+)
git switch feature-name

# Create and switch to new branch (modern)
git switch -c feature-name

# Delete branch
git branch -d feature-name

# Force delete branch
git branch -D feature-name
```

### Merging Branches
```bash
# Switch to target branch (usually main)
git checkout main

# Merge feature branch into current branch
git merge feature-name

# Delete merged branch
git branch -d feature-name
```

### Merge Types

**Fast-Forward Merge**: When target branch hasn't changed since branching
```bash
# Automatically happens when possible
git merge feature-branch
```

**Three-Way Merge**: When both branches have new commits
```bash
# Creates a merge commit
git merge feature-branch
```

**Squash Merge**: Combines all commits from feature branch into one
```bash
git merge --squash feature-branch
git commit -m "Add feature: description"
```

### Handling Merge Conflicts

When Git can't automatically merge changes:

1. Git marks conflicted files
2. Open files and look for conflict markers:
```
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name
```
3. Edit files to resolve conflicts
4. Remove conflict markers
5. Add resolved files: `git add filename`
6. Complete merge: `git commit`

---

## Remote Repositories

Remote repositories are versions of your project hosted elsewhere (like GitHub).

### Remote Commands
```bash
# View remote repositories
git remote -v

# Add remote repository
git remote add origin https://github.com/username/repo.git

# Change remote URL
git remote set-url origin https://github.com/username/new-repo.git

# Remove remote
git remote remove origin
```

### Fetching and Pulling
```bash
# Fetch changes from remote (doesn't merge)
git fetch origin

# Pull changes from remote (fetch + merge)
git pull origin main

# Pull with rebase instead of merge
git pull --rebase origin main
```

### Pushing Changes
```bash
# Push to remote repository
git push origin main

# Push new branch to remote
git push -u origin feature-branch

# Push all branches
git push --all origin

# Force push (dangerous!)
git push --force origin main
```

---

## GitHub Fundamentals

GitHub is a web-based platform that provides hosting for Git repositories plus additional collaboration features.

### Creating a GitHub Account
1. Go to [github.com](https://github.com)
2. Sign up for an account
3. Verify your email address

### Creating a Repository on GitHub
1. Click "New repository" button
2. Choose repository name
3. Add description (optional)
4. Choose public or private
5. Initialize with README (optional)
6. Add .gitignore template (optional)
7. Choose license (optional)

### Connecting Local Repository to GitHub
```bash
# Add GitHub repository as remote
git remote add origin https://github.com/username/repository.git

# Push existing repository
git push -u origin main
```

### Authentication

#### Personal Access Tokens (Recommended)
1. Go to GitHub Settings → Developer settings → Personal access tokens
2. Generate new token with appropriate permissions
3. Use token as password when prompted

#### SSH Keys (Alternative)
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copy public key to GitHub
cat ~/.ssh/id_ed25519.pub
# Paste in GitHub Settings → SSH and GPG keys
```

### GitHub Features

#### Issues
- Track bugs, feature requests, and tasks
- Assign to team members
- Add labels and milestones
- Reference in commits with `#issue-number`

#### Pull Requests
- Propose changes to repository
- Code review process
- Discussion and collaboration
- Automated testing integration

#### Wiki
- Documentation for your project
- Markdown support
- Version controlled

#### Releases
- Tag specific versions of your software
- Include release notes
- Attach binary files

---

## Collaboration Workflows

### Fork and Pull Request Workflow

This is the standard workflow for contributing to open source projects:

1. **Fork** the repository to your GitHub account
2. **Clone** your fork locally
```bash
git clone https://github.com/yourusername/repository.git
```
3. **Create** a new branch for your feature
```bash
git checkout -b feature-name
```
4. **Make** your changes and commit them
5. **Push** your branch to your fork
```bash
git push origin feature-name
```
6. **Create** a pull request on GitHub

### Keeping Your Fork Updated
```bash
# Add upstream remote
git remote add upstream https://github.com/original-owner/repository.git

# Fetch upstream changes
git fetch upstream

# Merge upstream changes into your main branch
git checkout main
git merge upstream/main

# Push updated main branch to your fork
git push origin main
```

### Team Collaboration Workflow

#### Centralized Workflow
- Everyone clones from central repository
- Push directly to main branch
- Good for small teams

#### Feature Branch Workflow
- Create feature branches for new work
- Merge back to main when complete
- Better organization and review process

#### Gitflow Workflow
- Structured branching model
- Separate branches for features, releases, and hotfixes
- More complex but very organized

---

## Advanced Git Features

### Stashing
Temporarily save changes without committing:
```bash
# Stash current changes
git stash

# Apply most recent stash
git stash pop

# List all stashes
git stash list

# Apply specific stash
git stash apply stash@{2}

# Create named stash
git stash push -m "work in progress on feature"

# Stash including untracked files
git stash -u
```

### Rebasing
Rewrite commit history by moving commits to a new base:
```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort
```

### Cherry-picking
Apply specific commits from one branch to another:
```bash
# Apply specific commit to current branch
git cherry-pick commit-hash

# Cherry-pick without committing
git cherry-pick -n commit-hash
```

### Tagging
Mark specific points in history:
```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0 release"

# List tags
git tag

# Push tags to remote
git push origin --tags

# Delete tag
git tag -d v1.0.0
git push origin --delete v1.0.0
```

### Reset and Revert
```bash
# Undo last commit (keep changes in working directory)
git reset --soft HEAD~1

# Undo last commit (unstage changes)
git reset HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Create new commit that undoes previous commit
git revert commit-hash
```

### Bisect
Find the commit that introduced a bug:
```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark known good commit
git bisect good commit-hash

# Git will check out commits for you to test
# Mark each as good or bad until bug is found
git bisect good  # or git bisect bad

# End bisect session
git bisect reset
```

---

## Best Practices

### Commit Messages
Follow the conventional format:
```
type(scope): brief description

Longer explanation if needed

- Use present tense ("Add feature" not "Added feature")
- Keep first line under 50 characters
- Use body to explain what and why, not how
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Examples:
- `feat(auth): add user login functionality`
- `fix(api): resolve null pointer exception in user service`
- `docs(readme): update installation instructions`

### Branching Strategy
- Use descriptive branch names: `feature/user-authentication`, `bugfix/login-error`
- Keep branches focused on single features or fixes
- Delete branches after merging
- Regularly update feature branches with main branch changes

### Repository Organization
- Include comprehensive README.md
- Add appropriate .gitignore file
- Use consistent file and folder naming
- Include license file for open source projects
- Add contributing guidelines for team projects

### Code Review Guidelines
- Review for functionality, not just syntax
- Be constructive and specific in feedback
- Test changes locally when possible
- Approve only when confident in changes
- Use draft pull requests for work in progress

---

## Troubleshooting

### Common Issues and Solutions

#### "Permission denied" when pushing
```bash
# Check remote URL
git remote -v

# Update to use HTTPS with token or SSH
git remote set-url origin https://github.com/username/repo.git
```

#### Accidentally committed to wrong branch
```bash
# Move last commit to new branch
git branch new-branch
git reset --hard HEAD~1
git checkout new-branch
```

#### Need to change last commit message
```bash
# Change most recent commit message
git commit --amend -m "New message"

# Change older commit messages
git rebase -i HEAD~n  # where n is number of commits back
```

#### Merge conflicts in binary files
```bash
# Choose your version
git checkout --ours filename

# Choose their version
git checkout --theirs filename

# Add resolved file
git add filename
```

#### Accidentally deleted branch
```bash
# Find the commit hash from reflog
git reflog

# Recreate branch from commit
git branch branch-name commit-hash
```

#### Large files causing issues
```bash
# Remove large file from history
git filter-branch --tree-filter 'rm -f large-file.zip' HEAD

# Use Git LFS for large files
git lfs track "*.zip"
git add .gitattributes
```

### Git Aliases
Make your life easier with shortcuts:
```bash
# Add to ~/.gitconfig or use git config --global
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

### Useful Commands for Investigation
```bash
# See who changed what in a file
git blame filename

# Search for text in commit messages
git log --grep="search term"

# Search for text in file contents
git log -S "search term"

# Show file changes over time
git log --follow filename

# Show branches that contain specific commit
git branch --contains commit-hash
```

---

## Conclusion

Git and GitHub are powerful tools that become more valuable as you master them. Start with the basics and gradually incorporate more advanced features as you become comfortable. The key to mastering Git is regular practice and understanding the underlying concepts of how Git tracks and manages changes.

Remember that Git is designed to be safe - it's very hard to permanently lose committed work. Don't be afraid to experiment and try new commands, especially in test repositories.

For more information, consult the official Git documentation at [git-scm.com/doc](https://git-scm.com/doc) and GitHub's help documentation at [docs.github.com](https://docs.github.com).

Happy coding and collaborating!