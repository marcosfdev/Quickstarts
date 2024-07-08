# QuickStarts

## Table of Contents
1. [SvelteQuickStart](./SvelteQuickStart/README.md) - Base SvelteKit Project with TypeScript, Tailwind CSS.
2. [Templar](./Templar/README.md) - Base SvelteKit Project with TypeScript, Tailwind CSS, dotenv, and enhanced error handling.
3. [Git Best Practices](./GitBestPractices/README.md) - Guidelines for branching and feature development.
4. [GCP and Firebase QuickStart](./GCPFirebaseQuickStart/README.md) - QuickStart for Google Cloud Platform and Firebase.
5. [AWS QuickStart](./AWSQuickStart/README.md) - QuickStart for Amazon Web Services.
6. [Supabase QuickStart](./Supa/README.md) - Quickstart for Supabase. 

## Quick Command to Add a New QuickStart

```bash
# Create a new directory for your QuickStart
mkdir NewQuickStart

# Navigate into the directory
cd NewQuickStart

# Initialize a new Git repository
git init

# Add a README.md file
echo "# NewQuickStart" > README.md

# Add all files and commit
git add .
git commit -m "Initial commit"

# Link to your GitHub repository
git remote add origin https://github.com/yourusername/QuickStarts.git

# Push your changes
git push -u origin main
