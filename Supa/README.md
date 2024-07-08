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

## Add Firebase for Push Notifications
To enable push notifications to be sent to iPHONe set up a new Firebase project.
1. Create new Firebase project
 ```bash
firebase init
```
2. Select Functions and press enter
3. Seelect create a new project
4. Name the project
5. Retype the name of the project
6. Select Typescript
7. Select Y to add ESLint
8. Select yes to install npm dependancies
9. Navigate to the functions directory and open the index.ts

### Sample node function for sending Push notifications example:
```bash
/functions/src/index.ts
const functions = require('firebase-functions');
const admin = require('firebase-admin');
const { FCM } = require('firebase-admin/messaging');

admin.initializeApp();

exports.sendNotification = functions.https.onCall((data, context) => {
  const message = {
    notification: {
      title: 'Sports Score Update',
      body: 'Your favorite team just scored!',
    },
    token: data.token, // Replace with the user's device token
  };

  return admin.messaging().send(message)
    .then((response) => {
      console.log('Successfully sent message:', response);
      return { success: true };
    })
    .catch((error) => {
      console.error('Error sending message:', error);
      return { success: false };
    });
});
```

10. Replace the user token above and save
11. Deploy the Function
Deploy: Use the Firebase CLI to deploy the function:
 ```bash
firebase deploy --only functions
```
12. Get Your FCM Server Key
- Firebase Console: Go to your Firebase project in the console.
- Project Settings: Click on "Project settings" in the left sidebar.
- Cloud Messaging: Select the "Cloud Messaging" tab.
- Server Key: Copy your FCM server key. You'll need this to send notifications.

### Collect User Device Tokens
13. In Your App: When users opt-in to receive notifications, use the Firebase SDK to collect their device tokens.

```bash
// src/routes/phone-number.svelte
<script lang="ts">
  import { onMount } from 'svelte';
  import { browser } from '$app/environment';

  let phoneNumber: string = '';
  let token: string | null = null;

  onMount(() => {
    if (browser) {
      // Initialize Firebase SDK for Web
      // Import the Firebase SDK for Web
import firebase from 'firebase/app';
import 'firebase/auth'; // Import the auth module
import 'firebase/messaging'; // Import the messaging module

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  databaseURL: "YOUR_DATABASE_URL",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID",
  measurementId: "YOUR_MEASUREMENT_ID" // Optional for analytics
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);

// Get a reference to the messaging service
const messaging = firebase.messaging();

// Get the user's device token
      firebase.messaging().getToken()
        .then((currentToken) => {
          if (currentToken) {
            token = currentToken;
            console.log('Token:', token);
          } else {
            console.error('No token received.');
          }
        })
        .catch((err) => {
          console.error('Error getting token:', err);
        });
    }
  });

  const handleSubmit = async () => {
    // Send the phone number and token to your server
    try {
      const response = await fetch('/api/users', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          phoneNumber,
          token,
        }),
      });

      if (response.ok) {
        // Handle successful submission
        console.log('Phone number and token submitted successfully!');
      } else {
        // Handle error
        console.error('Error submitting data:', response.status);
      }
    } catch (error) {
      console.error('Error submitting data:', error);
    }
  };
</script>

<form on:submit|preventDefault={handleSubmit}>
  <label for="phone-number">Phone Number:</label>
  <input type="tel" id="phone-number" bind:value={phoneNumber} />
  <button type="submit">Submit</button>
</form>
```
### Send Notifications

14. Call Your Function: From your app, make an HTTP request to your deployed Firebase function, passing the user's device token as a parameter. Example using Fetch:

```bash
fetch('https://us-central1-your-project-id.cloudfunctions.net/sendNotification', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ token: 'user_device_token' }),
})
.then(response => response.json())
.then(data => {
  if (data.success) {
    console.log('Notification sent successfully!');
  } else {
    console.error('Error sending notification.');
  }
})
.catch(error => {
  console.error('Error sending notification:', error);
});
```
    
16. Deploy the function
    ```bash
    firebase deploy

   
