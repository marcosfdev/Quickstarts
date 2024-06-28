# SvelteQuickStart

## Setup
```bash
bun create svelte@latest my-app
cd my-app
bun  install
```
### Add dependandcies:
```bash
bun install typescript @sveltejs/adapter-auto tailwindcss postcss autoprefixer dotenv winston @sentry/browser @sentry/tracing
```

## Configure Tailwind
```bash
bun tailwindcss init -p

/svelte.config.js

import { vitePreprocess } from "@sveltejs/vite-plugin-svelte";
import adapter from "@sveltejs/adapter-auto";

/** @type {import('@sveltejs/kit').Config} */
const config = {
  kit: {
    // adapter-auto only supports some environments, see https://kit.svelte.dev/docs/adapter-auto for a list.
    // If your environment is not supported or you settled on a specific environment, switch out the adapter.
    // See https://kit.svelte.dev/docs/adapters for more information about adapters.
    adapter: adapter(),
  },

  preprocess: [vitePreprocess({})],
};

export default config;
```
### Tailwind Configuration

```bash
/tailwind.config.js

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    extend: {}
  },
  plugins: [
    require('tailwindcss-plugins/pagination'),
    require('tailwindcss-plugins/gradients'),
    require('tailwindcss-plugins/animations'),
    require('tailwindcss-plugins/keyframes'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/forms'),
	  require('@tailwindcss/aspect-ratio'),	
    require('autoprefixer'),
  ]
};

```
### Add Tailwind to .css or .pcss
```bash 
/src/lib/assets/global.css:

@tailwind base;
@tailwind components;
@tailwind utilities;
```
### Import to layout
 
```bash
<script>
  import "$lib/assets/styles/global.css";
</script>

<slot />
```

### Style a page

```bash
<h1 class="text-3xl font-bold underline">
  Hello world!
</h1>

<style lang="postcss">
  :global(html) {
    background-color: theme(colors.gray.100);
  }
</style>
```

### Start dev server

```bash
bun  run dev
```


