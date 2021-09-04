- This is part 1/3 of a series of blog posts that showcase email and password based authentication using Expo and Firebase.
    - Part 1: Project Setup (you are here)
    - Part 2: Sign up, Email Verification, and Sign In (coming soon)
    - Part 3: Sign out, Forgot Password, Password Reset, and Conclusion (coming soon)

For a while now, I've been using Firebase as my go-to tool to quickly setup an email and password based authentication flow when working with Expo. While there are many other solutions out there, I've yet to find one that allows me to get started so quickly as Firebase does.

The goal of this series of blog posts, is to provide a simple example of how to setup Expo and Firebase with email and password based authentication. When done, this app will support sign up, sign in, sign out, email verification, forgot password, and password reset. All the code for this series of blog posts is available on [this Github repository](https://github.com/diegocasmo/expo-firebase-authentication). Let's jump right in.

## Create a New Expo App

If not done already, install the Expo CLI by running `npm install --global expo-cli` so that the application can be created. Next, create a new application by running `expo init --npm` and select the blank template. The application can now be served with `npm start` and opened locally or in the device/simulator of your preference. For simplicity, I'll be running it through [React Native for web](https://docs.expo.dev/workflow/web/),  which works out of the box with the Expo SDK. To do so, simply press `w` in the terminal that's running the Expo CLI.

Assuming everything went smoothly, the newly created Expo app should now be accessible via a web browser in [`http://localhost:19006/`](http://localhost:19006/), where it should display a familiar welcome message.

## Create a Firebase Project

Navigate to your Firebase account and create a new project. Once the project is created, click in the sidebar "Authentication" menu item, and enable the "Email/Password" provider.

Next, go to the project settings and register a web app for the project.

Finally, add the Firebase SDK to the web app (this configuration will later on be used to initialize the Firebase app in the Expo project).

## Configure Expo and Firebase

It's time to use the Firebase project configuration in the Expo app. To do so, first install the Firebase SDK by running `expo install firebase` . Additionally, install [react-native-dotenv](https://github.com/goatandsheep/react-native-dotenv) by running `npm install react-native-dotenv` , so that environment variables can be imported in the app via an `.env` file. Next, add the `module:react-native-dotenv` plugin to the default Expo `babel.config.js` file.

```jsx
module.exports = function(api) {
  api.cache(true);
  return {
    presets: ['babel-preset-expo'],
    plugins: [
      ['module:react-native-dotenv'], // Add this
    ],
  };
};
```

Create an `.env` file in the root directory of the project by running `touch .env`, and fill it in with the correct Firebase SDK configuration details. I've prefixed them with `FIREBASE_` so that it's clear what these are for.

```jsx
FIREBASE_API_KEY=
FIREBASE_AUTH_DOMAIN=
FIREBASE_PROJECT_ID=
FIREBASE_STORAGE_BUCKET=
FIREBASE_MESSAGING_SENDER_ID=
FIREBASE_APP_ID=
```

Create the app's API directory by running `mkdir -p src/api` and an index file within it with `touch src/api/index.js`. This is where the Firebase app will be initialized. To do so, use the environment variables.

```jsx
import firebase from 'firebase';
import {
  FIREBASE_API_KEY,
  FIREBASE_AUTH_DOMAIN,
  FIREBASE_PROJECT_ID,
  FIREBASE_STORAGE_BUCKET,
  FIREBASE_MESSAGING_SENDER_ID,
  FIREBASE_APP_ID,
} from '@env'

const firebaseConfig = {
  apiKey: FIREBASE_API_KEY,
  authDomain: FIREBASE_AUTH_DOMAIN,
  projectId: FIREBASE_PROJECT_ID,
  storageBucket: FIREBASE_STORAGE_BUCKET,
  messagingSenderId: FIREBASE_MESSAGING_SENDER_ID,
  appId: FIREBASE_APP_ID,
};

firebase.initializeApp(firebaseConfig);
```

Finally, import this file in `App.js` (located in the root directory):

```jsx
import './src/api'; // Import the Firebase API configuration file
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

// Rest of the file omitted...
```

## Application Structure and Dependencies

To simplify the application, I'll be using the following libraries (installation instructions are outlined below each of them). Note that none of these libraries are technically required, but they will simplify getting up and running.

- [NativeBase](https://docs.nativebase.io/install-expo): mobile-first, accessible components for React Native & Web.

    ```bash
    npm install native-base styled-components styled-system
    expo install react-native-svg
    expo install react-native-safe-area-context
    ```

- [Formik](https://formik.org/docs/overview#npm): create forms, manage, and validate their state.

    ```bash
    npm install formik
    ```

- [Yup](https://github.com/jquense/yup#install):  schema builder for value parsing and validation ([which integrates nicely with Formik](https://formik.org/docs/overview#complementary-packages)).

    ```bash
    npm install yup
    ```

- [React Navigation](https://reactnavigation.org/docs/getting-started/#installation): routing and navigation for Expo and React Native apps.

    ```bash
    npm install @react-navigation/native
    expo install react-native-screens react-native-safe-area-context
    ```

Let's now create the "application structure". The application structure will use a "feature-based" approach, where each feature (i.e., sign up, email verification, etc) will be contained within a directory.

```bash
src/
	Root.js
  api/ # API-only methods (Firebase)
  components/ # Re-usable components (among many features)
  features/
    sign-up/
      components/ # Components used by the sign-up feature only
      screens/ # Container type component(s) which setup layout/structure and higher level logic
```

The first step is to create the `Root.js` file. To do so, copy `App.js` by running `cp App.js src/Root.js`, then remove `App.js` (`rm App.js`), and update the `package.json` file so that `main` points to `src/Root.js`:

```bash
"main": "src/Root.js"
```

Next, replace the contents of `Root.js` so that it imports the Firebase API, setup the `native-base` provider, and register the root Expo component.

```jsx
import './api';
import { registerRootComponent } from 'expo';
import React from 'react';
import { StatusBar } from 'expo-status-bar';
import { NativeBaseProvider, Box } from 'native-base';

const Root = () => (
  <NativeBaseProvider>
    <Box>Hello world</Box>
    <StatusBar style="auto" />
  </NativeBaseProvider>
);

registerRootComponent(Root);
```

Start the Expo server once again by running `npm start` and make sure the "Hello world" message is correctly shown.

## Re-cap

In this blog post, we've gone over setting up Expo and Firebase together with the goal of implementing an email and password based authentication system. Additionally, a common set of libraries were installed and setup to make it easier to implement the upcoming features the app will need. In the next blog post, we'll implement the sign up, email verification, and sign in features.
