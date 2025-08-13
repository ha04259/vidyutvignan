# Complete Git and GitHub Learning Path

## Prerequisites
- Install Git on your system
- Create a GitHub account
- Set up a text editor (VS Code, Sublime, etc.)

---

## **Topic 1: Git Basics and Setup**

### Concepts to Learn:
- What is Git and version control
- Git vs GitHub
- Initial configuration

### Tasks:
1. **Install and Configure Git**
   ```bash
   # Check Git version
   git --version
   
   # Configure user information
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   
   # Check configuration
   git config --list
   ```

2. **Understanding Git Help**
   ```bash
   # Get help for any command
   git help init
   git --help add
   ```

---

## **Topic 2: Repository Initialization and Basic Operations**

### Concepts to Learn:
- Repository (repo) concept
- Working directory, staging area, and repository
- Basic Git workflow

### Tasks:
1. **Create Your First Repository**
   ```bash
   # Create a new directory
   mkdir my-first-repo
   cd my-first-repo
   
   # Initialize Git repository
   git init
   
   # Check repository status
   git status
   ```

2. **Create and Track Files**
   ```bash
   # Create a file
   echo "# My First Project" > README.md
   
   # Check status
   git status
   
   # Add file to staging area
   git add README.md
   
   # Check status again
   git status
   
   # Commit changes
   git commit -m "Initial commit: Add README"
   ```

---

## **Topic 3: File Operations and Staging**

### Concepts to Learn:
- Working directory vs staging area vs repository
- Different ways to add files
- File states in Git

### Tasks:
1. **Practice Different Add Commands**
   ```bash
   # Create multiple files
   echo "console.log('Hello World');" > app.js
   echo "body { margin: 0; }" > style.css
   echo "node_modules/" > .gitignore
   
   # Add specific file
   git add app.js
   
   # Add all files
   git add .
   
   # Check status
   git status
   
   # Commit changes
   git commit -m "Add JavaScript, CSS, and gitignore files"
   ```

2. **Practice Unstaging Files**
   ```bash
   # Modify a file
   echo "// Updated app" >> app.js
   
   # Stage the change
   git add app.js
   
   # Unstage the change
   git reset HEAD app.js
   
   # Check status
   git status
   ```

---

## **Topic 4: Commit History and Viewing Changes**

### Concepts to Learn:
- Commit history
- Viewing differences
- Commit messages best practices

### Tasks:
1. **View Commit History**
   ```bash
   # View commit history
   git log
   
   # View compact history
   git log --oneline
   
   # View graphical history
   git log --graph --oneline
   
   # View specific number of commits
   git log -3
   ```

2. **View Differences**
   ```bash
   # Modify a file
   echo "// Another update" >> app.js
   
   # View differences (unstaged)
   git diff
   
   # Stage the file
   git add app.js
   
   # View staged differences
   git diff --staged
   
   # Commit the change
   git commit -m "Update app.js with additional comment"
   ```

---

## **Topic 5: Branching Fundamentals**

### Concepts to Learn:
- What are branches
- Creating and switching branches
- Branch visualization

### Tasks:
1. **Create and Switch Branches**
   ```bash
   # View current branches
   git branch
   
   # Create a new branch
   git branch feature/login
   
   # Switch to the branch
   git checkout feature/login
   
   # Or create and switch in one command
   git checkout -b feature/signup
   
   # View all branches
   git branch
   ```

2. **Work on Different Branches**
   ```bash
   # On feature/signup branch, create a file
   echo "function signup() { }" > signup.js
   git add signup.js
   git commit -m "Add signup functionality"
   
   # Switch back to main
   git checkout main
   
   # Notice signup.js is not here
   ls
   
   # Switch back to feature/signup
   git checkout feature/signup
   ls
   ```

---

## **Topic 6: Merging Branches**

### Concepts to Learn:
- Fast-forward merge
- Three-way merge
- Merge conflicts

### Tasks:
1. **Fast-Forward Merge**
   ```bash
   # Switch to main branch
   git checkout main
   
   # Merge feature branch
   git merge feature/signup
   
   # View history
   git log --oneline
   
   # Delete merged branch
   git branch -d feature/signup
   ```

2. **Create Merge Conflict and Resolve**
   ```bash
   # Create two branches with conflicting changes
   git checkout -b feature/header
   echo "<h1>My App</h1>" > index.html
   git add index.html
   git commit -m "Add header"
   
   git checkout main
   git checkout -b feature/title
   echo "<h1>Welcome to My App</h1>" > index.html
   git add index.html
   git commit -m "Add title"
   
   # Merge one branch
   git checkout main
   git merge feature/header
   
   # Try to merge the other (conflict!)
   git merge feature/title
   
   # Resolve conflict by editing index.html
   # Then add and commit
   git add index.html
   git commit -m "Resolve merge conflict"
   ```

---

## **Topic 7: Remote Repositories and GitHub**

### Concepts to Learn:
- Local vs remote repositories
- GitHub basics
- Connecting local repo to GitHub

### Tasks:
1. **Create GitHub Repository**
   - Go to GitHub.com
   - Click "New Repository"
   - Name it "my-first-repo"
   - Don't initialize with README (we already have one)
   - Copy the repository URL

2. **Connect Local Repo to GitHub**
   ```bash
   # Add remote origin
   git remote add origin https://github.com/yourusername/my-first-repo.git
   
   # View remotes
   git remote -v
   
   # Push to GitHub
   git push -u origin main
   
   # Visit your GitHub repo to see the files
   ```

---

## **Topic 8: Push, Pull, and Fetch**

### Concepts to Learn:
- Pushing changes
- Pulling changes
- Fetching vs pulling

### Tasks:
1. **Push Changes to GitHub**
   ```bash
   # Make a change
   echo "## Features" >> README.md
   git add README.md
   git commit -m "Update README with features section"
   
   # Push to GitHub
   git push origin main
   ```

2. **Simulate Remote Changes and Pull**
   ```bash
   # Make a change directly on GitHub (edit a file through web interface)
   # Then pull those changes
   git pull origin main
   
   # View the changes
   git log --oneline
   ```

---

## **Topic 9: Cloning and Forking**

### Concepts to Learn:
- Cloning repositories
- Forking vs cloning
- Working with cloned repositories

### Tasks:
1. **Clone a Repository**
   ```bash
   # Clone your own repo to a different location
   cd ..
   git clone https://github.com/yourusername/my-first-repo.git cloned-repo
   cd cloned-repo
   
   # Make changes
   echo "console.log('Cloned repo');" >> app.js
   git add app.js
   git commit -m "Update from cloned repo"
   git push origin main
   ```

2. **Fork a Public Repository**
   - Go to a popular GitHub repo (e.g., microsoft/vscode)
   - Click "Fork" button
   - Clone your forked version
   - Make a small change
   - Push to your fork

---

## **Topic 10: Pull Requests**

### Concepts to Learn:
- What are pull requests
- Creating pull requests
- Code review process

### Tasks:
1. **Create Pull Request from Branch**
   ```bash
   # In your main repository, create a new branch
   git checkout -b feature/footer
   echo "<footer>© 2024 My App</footer>" > footer.html
   git add footer.html
   git commit -m "Add footer component"
   git push origin feature/footer
   ```
   - Go to GitHub
   - You'll see option to create Pull Request
   - Create PR with description
   - Merge the PR

2. **Create Pull Request to External Repository**
   - Use your forked repository
   - Make a meaningful change
   - Create pull request to original repository

---

## **Topic 11: Issues and Project Management**

### Concepts to Learn:
- GitHub Issues
- Labels and milestones
- Linking commits to issues

### Tasks:
1. **Create and Manage Issues**
   - Create new issue: "Add user authentication"
   - Add labels: "enhancement", "high priority"
   - Assign to yourself
   - Create milestone: "Version 1.0"

2. **Link Commits to Issues**
   ```bash
   # Reference issue in commit message
   git commit -m "Start user auth implementation (fixes #1)"
   # or
   git commit -m "Work on authentication for issue #1"
   ```

---

## **Topic 12: Advanced Git Operations**

### Concepts to Learn:
- Stashing changes
- Reverting commits
- Resetting commits
- Rebasing

### Tasks:
1. **Git Stash**
   ```bash
   # Make some changes but don't commit
   echo "// Work in progress" >> app.js
   
   # Stash changes
   git stash
   
   # Check status (changes are gone)
   git status
   
   # Apply stash
   git stash pop
   
   # List stashes
   git stash list
   ```

2. **Revert and Reset**
   ```bash
   # Make a commit
   echo "// Bad code" >> app.js
   git add app.js
   git commit -m "Add bad code"
   
   # Revert the commit
   git revert HEAD
   
   # Reset to previous commit (be careful!)
   git reset --soft HEAD~1
   ```

---

## **Topic 13: Git Workflows**

### Concepts to Learn:
- Feature branch workflow
- Gitflow workflow
- GitHub flow

### Tasks:
1. **Implement Feature Branch Workflow**
   ```bash
   # For each feature:
   git checkout main
   git pull origin main
   git checkout -b feature/new-feature
   # Make changes
   # Commit changes
   git push origin feature/new-feature
   # Create PR on GitHub
   # Merge and delete branch
   ```

---

## **Topic 14: GitHub Pages and Actions**

### Concepts to Learn:
- Static site hosting with GitHub Pages
- Basic GitHub Actions
- Continuous Integration

### Tasks:
1. **Deploy to GitHub Pages**
   ```bash
   # Create an HTML file
   echo "<!DOCTYPE html><html><body><h1>My Project</h1></body></html>" > index.html
   git add index.html
   git commit -m "Add index.html for GitHub Pages"
   git push origin main
   ```
   - Go to repository settings
   - Enable GitHub Pages from main branch
   - Visit your site at `https://yourusername.github.io/my-first-repo`

2. **Create Simple GitHub Action**
   ```bash
   # Create workflow directory
   mkdir -p .github/workflows
   
   # Create workflow file
   cat > .github/workflows/greet.yml << 'EOF'
   name: Greet
   on: [push]
   jobs:
     greet:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - name: Greet
           run: echo "Hello, GitHub Actions!"
   EOF
   
   git add .github/
   git commit -m "Add GitHub Action workflow"
   git push origin main
   ```

---

## **Topic 15: Collaboration Best Practices**

### Concepts to Learn:
- Code review guidelines
- Commit message conventions
- Branch naming conventions
- Documentation

### Tasks:
1. **Set Up Repository Standards**
   ```bash
   # Create CONTRIBUTING.md
   cat > CONTRIBUTING.md << 'EOF'
   # Contributing Guidelines
   
   ## Commit Messages
   - Use present tense: "Add feature" not "Added feature"
   - Keep first line under 50 characters
   - Reference issues when applicable
   
   ## Branch Naming
   - feature/description-of-feature
   - bugfix/description-of-bug
   - hotfix/description-of-hotfix
   EOF
   
   git add CONTRIBUTING.md
   git commit -m "Add contributing guidelines"
   git push origin main
   ```

---

## **Final Project: Complete Repository**

### Task: Create a Complete Project Repository
1. **Initialize a new repository** for a simple web project
2. **Set up proper structure** with folders for HTML, CSS, JS
3. **Create multiple branches** for different features
4. **Use issues** to track tasks
5. **Create pull requests** for each feature
6. **Set up GitHub Pages** for deployment
7. **Add GitHub Actions** for basic CI
8. **Write comprehensive README** with installation instructions
9. **Add proper gitignore** for your project type
10. **Practice the complete workflow** from development to deployment

---

## **Quick Reference Commands**

### Essential Git Commands
```bash
# Repository Setup
git init
git clone <url>

# Basic Operations
git status
git add <file>
git commit -m "message"
git push origin <branch>
git pull origin <branch>

# Branching
git branch
git checkout <branch>
git checkout -b <new-branch>
git merge <branch>

# History
git log
git diff
git show <commit>

# Remote Operations
git remote -v
git remote add origin <url>
git fetch
git push -u origin main
```

### GitHub Workflow
1. Fork/Clone repository
2. Create feature branch
3. Make changes and commit
4. Push branch to GitHub
5. Create Pull Request
6. Review and merge
7. Delete feature branch

---

## **Next Steps**
After completing this learning path, explore:
- Advanced Git features (interactive rebase, cherry-pick, bisect)
- Git hooks
- Advanced GitHub Actions
- GitHub CLI
- Git LFS for large files
- Semantic versioning and releases