# Sveltekit with Typescript, Tailwind CSS and logging and error handling witth Winston and Sentry


## Creating a project


```bash
# create a new project in the current directory
bun create svelte@latest

# create a new project in my-app
bun create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `bun install` (or `pnpm install` or `yarn`), start a development server:

```bash
bun run dev

# or start the server and open the app in a new browser tab
bun run dev -- --open
```

## Building

To create a production version of your app:

```bash
bun run build
```
You can preview the production build with `bun run preview`.

## 1. Install the dotenv, Winston and Sentry to add better logging and error handling

Make sure you have installed the necessary packages:

```bash
bun install dotenv winston @sentry/browser @sentry/tracing
```

# Post-Installation Steps for `winston`, `@sentry/browser`, and `@sentry/tracing`

After installing `winston`, `@sentry/browser`, and `@sentry/tracing`, follow these steps to set them up in your project.

## 2. Set Up Winston Logger
Create a logger utility file using winston.

Create Logger File
Create src/lib/logger.ts:

```bash
import { createLogger, format, transports } from 'winston';

export const logger = createLogger({
  level: 'info',
  format: format.combine(
    format.timestamp(),
    format.json()
  ),
  transports: [
    new transports.Console(),
    new transports.File({ filename: 'error.log', level: 'error' })
  ]
});
```
## 3. Set Up Sentry for Monitoring
Create Sentry Utility File
Create src/lib/sentry.ts:
```bash
import * as Sentry from '@sentry/browser';
import { BrowserTracing } from '@sentry/tracing';

Sentry.init({
  dsn: 'your_sentry_dsn',
  integrations: [new BrowserTracing()],
  tracesSampleRate: 1.0
});

export { Sentry };
```

## 4. Integrate Logger and Sentry with SvelteKit
Update Global Error Handling
Modify src/hooks.server.ts to incorporate both winston and sentry:

```bash
import { Sentry } from './lib/sentry';
import { logger } from './lib/logger';

export const handle = async ({ event, resolve }) => {
  try {
    return await resolve(event);
  } catch (error) {
    Sentry.captureException(error);
    logger.error('Server error:', error);

    return new Response('Internal Server Error', { status: 500 });
  }
};
```
## 5. Environment Variables
Ensure you have a .env file in your project's root directory with the required environment variables. Example:

```bash
RAPID_API_KEY=your_rapid_api_key
RAPID_API_URL=your_rapid_api_url
SENTRY_DSN=your_sentry_dsn
```
## 6. Tracing Errors with Winston
### Locating Logs
Winston logs errors to both the console and a file (error.log). You can find the error.log file in the root of your project directory.

### Example CLI Commands for Logs
To view the logs directly from the console:

```bash
# Display the last 10 lines of the error log
tail -n 10 error.log

# Continuously watch the log file for new entries
tail -f error.log
```
## 7. Tracing Errors with Sentry
### Accessing Sentry
Login to Sentry: Visit Sentry.io and log in to your account.
Project Dashboard: Navigate to your project dashboard to view recent errors, issues, and performance metrics.

### Detailed Error Reports
Sentry provides detailed error reports including stack traces, breadcrumbs, and contextual data, making it easier to trace and debug errors.

### Example Sentry Error Capture
Sentry captures errors automatically if configured properly. Here’s an example of manually capturing an error:

```bash
import { Sentry } from './lib/sentry';

try {
  // Some code that may throw an error
} catch (error) {
  Sentry.captureException(error);
}
```
## 8. Full Example: Resolving an Internal Server 500 Error

### Scenario
You encounter a 500 Internal Server Error when trying to fetch data from an API endpoint.

### Steps to Resolve
1. Check the Browser Console:

Look for any error messages or stack traces that might give you a clue about the error.

2. Check the Server Logs:

Use the following command to view the recent entries in the error log:
```bash
tail -n 10 error.log
```
You might see an entry like this:

```bash
json
{
  "level": "error",
  "message": "Server error: Error: Failed to fetch data",
  "timestamp": "2024-06-28T12:34:56.789Z"
}
```
3. Investigate the Code:

Based on the error message, identify the code causing the issue. For example, it might be in src/lib/api.ts:

```bash
import { API_URL, API_KEY } from '$env/static/private';

export const fetchData = async (endpoint: string) => {
  try {
    const response = await fetch(`${API_URL}/${endpoint}`, {
      headers: {
        'x-rapidapi-key': API_KEY
      }
    });

    if (!response.ok) {
      throw new Error(`Error: ${response.statusText}`);
    }

    return await response.json();
  } catch (error) {
    console.error('API fetch error:', error);
    throw error;
  }
};
```
4. Add more logging:
Add more detailed logging to understand the issue better.

 ```bash
typescript
export const fetchData = async (endpoint: string) => {
  try {
    console.log(`Fetching data from ${API_URL}/${endpoint}`);
    const response = await fetch(`${API_URL}/${endpoint}`, {
      headers: {
        'x-rapidapi-key': API_KEY
      }
    });

    if (!response.ok) {
      logger.error(`Fetch failed with status: ${response.status}`);
      throw new Error(`Error: ${response.statusText}`);
    }

    const data = await response.json();
    console.log('Data fetched successfully:', data);
    return data;
  } catch (error) {
    console.error('API fetch error:', error);
    logger.error('API fetch error:', error);
    Sentry.captureException(error);
    throw error;
  }
};
```
5. Deploy and Test:

Deploy the updated code and test the API endpoint again. Monitor the console and logs for detailed error information.

6. Review Sentry Reports:

Check Sentry for detailed error reports, which include stack traces and additional context that can help identify the root cause.

# Additional Resources

- [SvelteKit Documentation](https://kit.svelte.dev/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [dotenv Documentation](https://github.com/motdotla/dotenv)
- [Winston Documentation](https://github.com/winstonjs/winston)
- [Sentry Documentation](https://docs.sentry.io/platforms/javascript/)


> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
