
### Templar README.md

# Templar

This folder contains a base SvelteKit project setup with TypeScript, Tailwind CSS, dotenv, and enhanced error handling. 
````markdown
## Project Structure
Templar/
│   ├── src/
│   │   ├── lib/
│   │   │--── utils/
│   │   │       ├── api.ts
│   │   │       ├── logger.ts
│   │   │       └── sentry.ts
|   |   |   ├── components/
|   |   |   │   ├── Header.svelte
|   |   |   │   ├── Card.svelte
|   |   |   │   ├── Footer.svelte
│   │   ├── routes/
│   │   │   ├── +layout.svelte
│   │   │   └── +error.svelte
│   │   ├── app.css
│   │   ├── env.ts
│   │   └── hooks.server.ts
│   ├── tailwind.config.cjs
│   ├── postcss.config.cjs
│   ├── .env.example
│   ├── package.json
│   └── README.md
├── GitBestPractices/
│   └── README.md
├── GCPFirebaseQuickStart/
│   └── README.md
├── AWSQuickStart/
│   └── README.md
└── README.md
````

## Setup Instructions
```bash
git clone https://github.com/marcosfdev/Templar.git
```
### Install Dependencies

```bash
bun install
```
### Git Setup
```bash
# Create and switch to a new feature branch
git checkout -b feature/your-feature-name
```

# Add changes and commit
```bash
git add .
git commit -m "Commit message"
# Sync with the main branch
git fetch origin
git checkout main
git pull origin main
git checkout feature/your-feature-name
git merge main
```
# Push feature branch to remote
```bash
git push origin feature/your-feature-name
```
# After merging the pull request
```bash
git checkout main
git pull origin main
```

### Run the Project
```bash
bun run dev
```

## Additional Resources

- [SvelteKit Documentation](https://kit.svelte.dev/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [dotenv Documentation](https://github.com/motdotla/dotenv)
- [Winston Documentation](https://github.com/winstonjs/winston)
- [Sentry Documentation](https://docs.sentry.io/platforms/javascript/) 
