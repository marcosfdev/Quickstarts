# Supabase
Quickstart for a Supabase DB app with push notifications with Sveltekit and Tailwind CSS.

## Example Integration with SvelteKit:
1. Set Up Supabase:

- Sign into Supabase and create a new project.
- Set up the  database schema to handle sports data (teams, leagues, matches, scores, etc.).

2. Install Supabase Client:

   ```bash
   bun install @supabase/supabase-js
   ```
3. Initialize Supabase in SvelteKit:
   ```bash
   import { createClient } from '@supabase/supabase-js';

   const supabaseUrl = 'https://your-project-id.supabase.co';
   const supabaseKey = 'your-anon-key';

   export const supabase = createClient(supabaseUrl, supabaseKey);

4. Fetch Data in SvelteKit:
```bash
// src/routes/+page.js
import { supabase } from '$lib/supabase';

export async function load() {
  const { data, error } = await supabase
    .from('scores')
    .select('*')
    .order('date', { ascending: false });

  if (error) {
    console.error(error);
    return { scores: [] };
  }

  return { scores: data };
}
```
5.  Display Data with Tailwind CSS:
   
```bash <!-- src/routes/+page.svelte -->
<script context="module" lang="ts">
  export { load } from './+page.js';
</script>

<script>
  export let scores;
</script>

<div class="container mx-auto px-4">
  <h1 class="text-3xl font-bold mb-4">Sports Scores</h1>
  {#each scores as score}
    <div class="mb-2 p-4 bg-white rounded shadow">
      <h2 class="text-xl">{score.team1} vs {score.team2}</h2>
      <p>Score: {score.score1} - {score.score2}</p>
      <p>Date: {new Date(score.date).toLocaleDateString()}</p>
    </div>
  {/each}
</div>
``` 
   
