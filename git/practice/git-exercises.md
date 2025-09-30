# Git Practice: Hands-On Exercises

## Exercise 1: Getting Started with Git

### Step 1: Install Git

**Windows:**
1. Go to https://git-scm.com/download/win
2. Download and run the installer
3. Follow the default settings

**Mac:**
```bash
# Open Terminal and run:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install git
```

### Step 2: Configure Git

Open Terminal/Command Prompt and tell Git who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

**Verify configuration:**
```bash
git config --list
```

---

## Exercise 2: Your First Repository

### Create Your First Repository

1. **Create a new folder for your project:**
```bash
mkdir my-first-git-project
cd my-first-git-project
```

2. **Initialize Git in that folder:**
```bash
git init
```

3. **Create a simple README file:**
```bash
echo "# My First Git Project" > README.md
echo "" >> README.md
echo "This is my first project using Git version control." >> README.md
```

---

## Exercise 3: Basic Git Workflow

### The Add-Commit-Push Cycle

1. **Check what files Git sees:**
```bash
git status
```

2. **Add your file to staging:**
```bash
git add README.md
# or add all files: git add .
```

3. **Commit (save) your changes:**
```bash
git commit -m "Add initial README file"
```

4. **Check the commit history:**
```bash
git log --oneline
```

---

## Exercise 4: Making Changes

### Edit and Track Changes

1. **Edit your README.md file:**
```bash
echo "" >> README.md
echo "## Features" >> README.md
echo "- Version control with Git" >> README.md
echo "- Learning basic commands" >> README.md
```

2. **Check what changed:**
```bash
git status
git diff
```

3. **Stage and commit the changes:**
```bash
git add README.md
git commit -m "Add features section to README"
```

---

## Exercise 5: Working with Branches

### Create and Manage Branches

1. **See current branches:**
```bash
git branch -a
```

2. **Create a new feature branch:**
```bash
git checkout -b feature-contact-section
```

3. **Add a contact section to README:**
```bash
echo "" >> README.md
echo "## Contact" >> README.md
echo "For questions, reach out to: your.email@example.com" >> README.md
```

4. **Commit the new feature:**
```bash
git add README.md
git commit -m "Add contact section"
```

5. **Switch back to main branch:**
```bash
git checkout main
```

6. **Merge the feature branch:**
```bash
git merge feature-contact-section
```

---

## Exercise 6: Connecting to GitHub

### Push to Remote Repository

1. **Create a GitHub repository:**
   - Go to https://github.com
   - Click "New repository"
   - Name it: `my-first-git-project`
   - Make it Public
   - Don't initialize with README

2. **Add the remote repository:**
```bash
git remote add origin https://github.com/YOUR_USERNAME/my-first-git-project.git
```

3. **Push your code:**
```bash
git push -u origin main
```

---

## Exercise 7: Collaboration Workflow

### Simulate Team Collaboration

1. **Create a new branch for a team feature:**
```bash
git checkout -b team-feature-navbar
```

2. **Add a navbar section:**
```bash
echo "" >> README.md
echo "## Navigation" >> README.md
echo "- [Home](#home)" >> README.md
echo "- [Features](#features)" >> README.md
echo "- [Contact](#contact)" >> README.md
```

3. **Commit the team feature:**
```bash
git add README.md
git commit -m "Add navigation section for team feature"
```

4. **Push the branch:**
```bash
git push -u origin team-feature-navbar
```

5. **Create a pull request on GitHub** (do this in your browser)

---

## Exercise 8: Fixing Mistakes

### Common Recovery Scenarios

**Undo last commit (keep changes):**
```bash
# Make a commit you want to undo
echo "This is a mistake" >> README.md
git add README.md
git commit -m "Mistake commit"

# Undo it but keep the changes
git reset --soft HEAD~1
```

**Undo last commit (delete changes):**
```bash
# Undo and delete the changes
git reset --hard HEAD~1
```

**Discard uncommitted changes:**
```bash
# Edit a file
echo "temporary change" >> README.md

# Discard the changes
git checkout -- README.md
```

---

## Exercise 9: Advanced Branching

### Complex Branching Scenarios

1. **Create multiple feature branches:**
```bash
git checkout -b feature-user-auth
git checkout main
git checkout -b feature-dashboard
git checkout main
git checkout -b bug-fix-mobile-layout
```

2. **See all branches:**
```bash
git branch -a
```

3. **Merge one feature:**
```bash
git checkout feature-user-auth
echo "## User Authentication" >> README.md
echo "- Login system" >> README.md
echo "- Password reset" >> README.md
git add README.md
git commit -m "Add user auth feature"

git checkout main
git merge feature-user-auth
```

---

## Exercise 10: Working with Remotes

### Sync with Remote Repository

**Pull latest changes:**
```bash
git pull origin main
```

**Fetch without merging:**
```bash
git fetch origin
git log --oneline origin/main
```

**See remote information:**
```bash
git remote -v
git remote show origin
```

---

## Practice Project: Build a Git-Tracked Website

### Complete Website Development

1. **Create project structure:**
```bash
mkdir my-website
cd my-website
git init

# Create basic files
echo "<!DOCTYPE html>" > index.html
echo "<html><head><title>My Site</title></head><body><h1>Welcome!</h1></body></html>" >> index.html

echo "body { font-family: Arial; margin: 40px; }" > styles.css

mkdir js
echo "console.log('Site loaded!');" > js/app.js
```

2. **Initial commit:**
```bash
git add .
git commit -m "Initial website setup"
```

3. **Add features in branches:**
```bash
# Navigation feature
git checkout -b feature-navigation
echo '<nav><a href="#home">Home</a> <a href="#about">About</a></nav>' >> index.html
git add index.html
git commit -m "Add navigation menu"

# Styling improvements
git checkout main
git checkout -b feature-styling
echo "nav { background: #333; color: white; padding: 10px; }" >> styles.css
git add styles.css
git commit -m "Improve navigation styling"
```

4. **Merge features:**
```bash
git checkout main
git merge feature-navigation
git merge feature-styling
```

---

## Common Git Commands Reference

### Daily Commands
```bash
git status           # Check current state
git add .            # Stage all changes
git commit -m "msg"  # Commit with message
git push             # Push to remote
git pull             # Pull from remote
```

### Branch Commands
```bash
git branch           # List branches
git checkout -b name # Create and switch branch
git merge branch     # Merge branch into current
git branch -d name   # Delete branch
```

### History Commands
```bash
git log              # See commit history
git log --oneline    # Compact history
git diff             # See unstaged changes
git diff --staged    # See staged changes
```

### Undo Commands
```bash
git reset --soft HEAD~1  # Undo commit, keep changes
git reset --hard HEAD~1  # Undo commit, delete changes
git checkout -- file     # Discard file changes
```

---

## Troubleshooting Common Issues

### "fatal: not a git repository"
- You're not in a Git-initialized folder
- Run `git init` first

### "nothing to commit, working tree clean"
- No changes have been made to tracked files
- Check `git status` to see current state

### "failed to push some refs"
- Remote has changes you don't have
- Run `git pull` first

### "Merge conflict"
- Two branches changed the same lines
- Edit conflicted files, then `git add` and `git commit`

---

## Next Steps

1. **Practice Daily**: Spend 15-30 minutes daily with Git
2. **Contribute**: Find open source projects on GitHub
3. **Learn Advanced**: Study rebasing, cherry-picking, and Git hooks
4. **Use Git GUI**: Try GitKraken or VS Code Git integration

Remember: Git mastery comes from practice. Don't be afraid to experiment in test repositories!
