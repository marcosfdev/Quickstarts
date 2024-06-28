
### GitBestPractices README.md

# Git Best Practices

This folder contains guidelines for branching and feature development.

## Branching Strategy

1. **Main Branch:** The main branch contains the stable codebase.
2. **Feature Branches:** Use feature branches for new features or improvements. Name them descriptively, e.g., `feature/your-feature-name`.

## Commands for Feature Development

# Create and switch to a new feature branch
git checkout -b feature/your-feature-name

# Add changes and commit
git add .
git commit -m "Commit message"

# Sync with the main branch
git fetch origin
git checkout main
git pull origin main
git checkout feature/your-feature-name
git merge main

# Push feature branch to remote
git push origin feature/your-feature-name

# After merging the pull request
git checkout main
git pull origin main
# GitBestPracties
