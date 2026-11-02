OUR LITTLE WORLD — setup guide

This is a starter website for your shared photo and video memories.

1. In Firebase Console, open your project.
2. Add a Web App (Project Overview > Add app > Web).
3. Copy the Firebase configuration object into index.html, replacing the six PASTE_ values.
4. In Authentication > Sign-in method, enable Email/Password.
5. In Firestore Database, create a database.
6. In Storage, get started and create the default bucket.
7. Add these Firestore rules (replace the rules in the Rules tab):

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /memories/{memoryId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null
                    && request.resource.data.ownerUid == request.auth.uid;
      allow update, delete: if request.auth != null
                            && resource.data.ownerUid == request.auth.uid;
    }
  }
}

8. Add these Storage rules (Storage > Rules):

rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /memories/{userId}/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null
                   && request.auth.uid == userId
                   && request.resource.size < 100 * 1024 * 1024;
    }
  }
}

IMPORTANT:
- These starter rules allow any authenticated account to read the gallery. For a truly private two-person site, create the two accounts you intend to use and then restrict read access to their exact email addresses or UIDs before publishing.
- Do not publish your Firebase Admin/private service-account keys. The web config is designed for browser use; security comes from Authentication and Rules.
- The two Google Drive album buttons open the folders you shared earlier. This starter does not automatically import Drive files into Firebase.
- Run the site locally with a simple static server, or deploy the folder using Firebase Hosting.
