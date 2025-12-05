# WorkLink

WorkLink is a micro-jobs marketplace that connects people in local and low-income communities with nearby paid tasks, such as cleaning, repairs, errands, and shop assistance. Busy individuals and small businesses can quickly find trusted, affordable help, while workers can access short-term gigs through web, WhatsApp, and USSD, even without smartphones or constant internet. Over time, WorkLink helps workers build a digital reputation and earning history they can use for better opportunities and financial inclusion.

## Features

- **Micro-jobs marketplace:** WorkLink connects people in local and low-income communities with nearby paid tasks, such as cleaning, repairs, errands, and shop assistance.

## Switching backend from localStorage to Firebase

This repo currently stores demo data in the browser `localStorage`. To move to a cloud backend using Firebase Firestore:

1. Create a Firebase project in the Firebase Console.
2. Enable Firestore in the project (start in test mode for development).
3. Copy `public/assets/js/firebase-config.example.js` to `public/assets/js/firebase-config.js` and fill in your project's config values.
4. (Optional) For local development, install and run the Firebase Emulator and set `window.__FIRESTORE_EMULATOR__` in your `firebase-config.js`.
5. Include `firebase-config.js` before `firebase-init.js` in your HTML (see `public/account.html` where `firebase-init.js` is loaded as a module).
6. Migrate functions in `public/assets/js/tasks.js` (and other files) to call `window.FB` async methods instead of `localStorage` helpers. The `window.FB` object provides:
	- `FB.available` boolean
	- `FB.getAll(collectionName)` -> returns array of documents
	- `FB.getDoc(collectionName, id)` -> single doc
	- `FB.add(collectionName, data)` -> adds a document
	- `FB.set(collectionName, id, data)` -> set/merge document
	- `FB.update(collectionName, id, patch)` -> partial update
	- `FB.delete(collectionName, id)` -> delete document
	- `FB.queryEqual(collectionName, field, value)` -> query by equality

Notes:
- The current `tasks.js` code uses synchronous operations with `localStorage`. Migrating to Firestore requires changing those functions to async/await and calling the `FB` wrapper. You can migrate incrementally by keeping localStorage as a fallback when `FB.available` is false.
- If you'd like, I can perform the migration for the main flows (users, tasks, applications) as a follow-up.

