---
title: Firebase
description: 'Discover Firebase preset for Nitro!'
---

**Preset:** `firebase` ([switch to this preset](/deploy/#changing-the-deployment-preset))

Nitro supports [Firebase Hosting](https://firebase.google.com/docs/hosting) with Cloud Functions out of the box.

**Note**: You need to be on the **Blaze plan** to use Nitro with Cloud Functions.

If you don't already have a `firebase.json` in your root directory, Nitro will create one the first time you run it. In this file, you will need to replace `<your_project_id>` with the ID of your Firebase project.

This file should then be committed to version control. You can also create a `.firebaserc` file if you don't want to manually pass your project ID to your `firebase` commands (with `--project <your_project_id>`):

```json [.firebaserc]
{
  "projects": {
    "default": "<your_project_id>"
  }
}
```

Then, just add Firebase dependencies to your project:

```bash
# yarn
yarn add --dev firebase-admin firebase-functions firebase-functions-test

# npm
npm install -D firebase-admin firebase-functions firebase-functions-test
```

### Using Firebase CLI

You may instead prefer to set up your project with the Firebase CLI, which will fetch your project ID for you, add required dependencies (see above) and even set up automated deployments via GitHub Actions.

#### Install Firebase CLI globally

```bash
# yarn
yarn global add firebase-tools

# npm
npm install -g firebase-tools
```


#### Initialize your Firebase project

```bash
firebase login
firebase init hosting
```

When prompted, you can enter `.output/public` as the public directory. In the next step, **do not** configure your project as a single-page app.

Once complete, add the following to your `firebase.json` to enable server rendering in Cloud Functions:

```json [firebase.json]
{
  "functions": { "source": ".output/server" },
  "hosting": [
    {
      "site": "<your_project_id>",
      "public": ".output/public",
      "cleanUrls": true,
      "rewrites": [{ "source": "**", "function": "server" }]
    }
  ]
}
```

You can find more details in the [Firebase documentation](https://firebase.google.com/docs/hosting/quickstart).

## Local preview

You can preview a local version of your site if you need to test things out without deploying.

```bash
NITRO_PRESET=firebase yarn build
firebase emulators:start
```

## Deploy to Firebase Hosting via CLI

Deploying to Firebase Hosting is a simple matter of just running the `firebase deploy` command.

```bash
NITRO_PRESET=firebase yarn build
firebase deploy
```
