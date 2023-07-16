# Firebase React Website Deployment Guide

This guide will walk you through the process of setting up and deploying a Firebase React website.

## Prerequisites

Before you begin, make sure you have the following:

- [Node.js](https://nodejs.org/en) is installed on your machine.
- Firebase CLI installed globally. If not, you can install it using the following command:

`npm install -g firebase-tools`

## Step 1: Clone the GitHub Repository

1. Open your terminal and navigate to the desired directory where you want to clone the repository.
2. Run the following command to clone the GitHub repository:

`git clone https://github.com/MScheiterle/ReactFirebaseTemplate.git`

## Step 2: Set up a Firebase Project

1. Go to the [Firebase Console](https://console.firebase.google.com/) and create a new project.
2. Click on "Add app" and select "Web" to add a new web app to your project.
3. Follow the instructions to register your app and obtain your Firebase configuration object.

## Step 3: Configure Firebase in your React App

1. Create a file in the main project directory named `.env.local`. This will prevent sensative project information from being uploaded to github.
2. Paste each token of the firebase configuration using the following format

```
REACT_APP_FIREBASE_API_KEY = YOUR_API_KEY
REACT_APP_FIREBASE_AUTH_DOMAIN = YOUR_AUTH_DOMAIN
REACT_APP_FIREBASE_PROJECT_ID = YOUR_PROJECT_ID
REACT_APP_FIREBASE_STORAGE_BUCKET = YOUR_STORAGE_BUCKET
REACT_APP_FIREBASE_MESSAGING_SENDER_ID = YOUR_MESSAGING_SENDER_ID
REACT_APP_FIREBASE_APP_ID = YOUR_APP_ID
REACT_APP_FIREBASE_MEASURE_ID = YOUR_MEASURE_ID
```

## Step 4: Customize your React App

1. Initialize your Firebase project by running the following command:

`firebase init`

- Select the Firebase features you want to set up.
- Choose your Firebase project from the list.
- Specify your build directory as build.
- Configure as a single-page if you only want one page.
- Overwrite index.html: No.

2. Replace all code within the `firebase.js` file with the following code:

```
{
  "hosting": {
    "public": "build",
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ],
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"]
  },
  "emulators": {
    "hosting": {
      "port": 5000
    },
    "ui": {
      "enabled": true
    },
    "singleProjectMode": true
  }
}
```

## Step 5: Deploy to Firebase

Use one of the below options to deploy the React App to your Firebase project

- Deploy your app to Firebase Hosting with the following command:

Although I prefer to use the Github option I added this one for all those who dont prefer Github, I am **NOT** certain if `.env.local` enviorment varibles will be transfered to the Firebase machine so if your `firebase.ts` file isnt working or you get an error in console saying it doesnt have an API_KEY that may be why.

`npm run deploy`

- Push to Github

This will only work if you added github workflows to the React App when you ran the Firebase init command

Within the workflow files `firebase-hosting-merge.yml` and `firebase-hosting-pull-request.yml` add the following above the jobs section:

```
env:
  REACT_APP_FIREBASE_API_KEY: ${{ secrets.REACT_APP_FIREBASE_API_KEY }}
  REACT_APP_FIREBASE_APP_ID: ${{ secrets.REACT_APP_FIREBASE_APP_ID }}
  REACT_APP_FIREBASE_AUTH_DOMAIN: ${{ secrets.REACT_APP_FIREBASE_AUTH_DOMAIN }}
  REACT_APP_FIREBASE_MEASURE_ID: ${{ secrets.REACT_APP_FIREBASE_MEASURE_ID }}
  REACT_APP_FIREBASE_MESSAGING_SENDER_ID: ${{ secrets.REACT_APP_FIREBASE_MESSAGING_SENDER_ID }}
  REACT_APP_FIREBASE_PROJECT_ID: ${{ secrets.REACT_APP_FIREBASE_PROJECT_ID }}
  REACT_APP_FIREBASE_STORAGE_BUCKET: ${{secrets.REACT_APP_FIREBASE_STORAGE_BUCKET }}
```

Finally inside of your github project you must add the enviorment secrects

1. Go to your Github account and naviagte to your projects repository

2. Find `Settings` then `Secrets and variables`

3. Now add the following to the `Repository Secrets` using the bright green `New repository secret` button in the top right, make sure to **NOT** add them to `Environment secrets`

- name: `REACT_APP_FIREBASE_API_KEY` secret: `YOUR_API_KEY`

- name: `REACT_APP_FIREBASE_AUTH_DOMAIN` secret: `YOUR_AUTH_DOMAIN`

- name: `REACT_APP_FIREBASE_PROJECT_ID` secret: `YOUR_PROJECT_ID`

- name: `REACT_APP_FIREBASE_STORAGE_BUCKET` secret: `YOUR_STORAGE_BUCKET`

- name: `REACT_APP_FIREBASE_MESSAGING_SENDER_ID` secret:  `YOUR_MESSAGING_SENDER_ID`

- name: `REACT_APP_FIREBASE_APP_ID` secret: `YOUR_APP_ID`

- name: `REACT_APP_FIREBASE_MEASURE_ID` secret: `YOUR_MEASURE_ID`

## Conclusion and Disclaimer

Just so its out there, when I say something like `YOUR_API_KEY`, I mean you should change that to your Firebase API_KEY given to you in the Firebase project config.