+++
date = "2024-06-22"
title = "Store additional user information in firebase using react and javascript"
slug = "store-additional-user-information-in-firebase-using-react-and-javascript"
tags = ["React", "JavaScript", "Firebase"]
+++

To store additional information for a user in `Firebase` using React and JavaScript, you typically use Firestore or the Realtime Database, as Firebase Authentication does not allow for arbitrary additional data to be stored directly in user profiles. Here’s a step-by-step guide on how to achieve this using Firestore.

## Step 1: Set Up Firebase in Your React App

First, ensure that you have Firebase set up in your React application. You need to install Firebase and initialize it in your project.

```bash
npm install firebase
```

Then, create a file (e.g., `firebase.js`) to initialize Firebase:

```javascript
import { initializeApp } from 'firebase/app';
import { getFirestore } from 'firebase/firestore';

const firebaseConfig = {
  apiKey: 'YOUR_API_KEY',
  authDomain: 'YOUR_AUTH_DOMAIN',
  projectId: 'YOUR_PROJECT_ID',
  storageBucket: 'YOUR_STORAGE_BUCKET',
  messagingSenderId: 'YOUR_MESSAGING_SENDER_ID',
  appId: 'YOUR_APP_ID',
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

// Export Firestore
export const db = getFirestore(app);
```

## Step 2: Create a Firestore Collection for Users

Next, create a Firestore collection where you will store additional user information. For example, you can create a `users` collection.

## Step 3: Save Additional User Data

When a user signs up or logs in, you can store their additional information in the Firestore database. Here’s how you can do it:

### Example Function to Create or Update User Data

```javascript
import { doc, setDoc } from 'firebase/firestore';
import { db } from './firebase'; // Adjust the import based on your file structure

async function saveUserData(user) {
  const userRef = doc(db, 'users', user.uid); // Use the user's UID as the document ID

  // Create or update the user document
  await setDoc(
    userRef,
    {
      displayName: user.displayName,
      email: user.email,
      phoneNumber: user.phoneNumber,
      gender: user.gender,
      birthday: user.birthday,
    },
    { merge: true }
  ); // Merge allows adding new fields without overwriting existing ones
}
```

## Step 4: Call the Function After User Authentication

You can call the `saveUserData` function after the user signs in or signs up. Here’s an example of how to integrate it within your authentication flow:

```javascript
import { getAuth, onAuthStateChanged } from 'firebase/auth';

// Initialize Firebase Auth
const auth = getAuth();

onAuthStateChanged(auth, async (user) => {
  if (user) {
    // User is signed in
    await saveUserData(user);
  } else {
    // User is signed out
  }
});
```

## Summary

By following these steps, you can effectively store additional user information in Firebase using Firestore. This approach allows you to maintain a flexible structure for user data that can grow as your application requirements change. You can also retrieve this data later by querying the `users` collection in Firestore as needed
