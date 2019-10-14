# nuxt-spa

The easiest way to create and deploy to firebase a nuxt app

# Requirements
npm or yarn

# Setup
First let's look at what we have to setup. These are things you only need to do once.
## 1. Create Nuxt App
Into your project directory, run nuxt cli.  
In my case I'm using vs code's terminal  
You can see my setup below, I recommend you not to use eslint at this point (if you don't want to start by fixing all kinds of issues)
```console
$ npx create-nuxt-app
npx : 350 installé(s) en 32.227s

create-nuxt-app v2.11.1
✨  Generating Nuxt.js project in .
? Project name nuxt-spa
? Project description My doozie Nuxt.js project
? Author name sam-eah
? Choose the package manager Yarn
? Choose UI framework Vuetify.js
? Choose custom server framework None (Recommended)
? Choose Nuxt.js modules (Press <space> to select, <a> to toggle all, <i> to invert selection)
? Choose linting tools (Press <space> to select, <a> to toggle all, <i> to invert selection)
? Choose test framework None
? Choose rendering mode Single Page App
? Choose development tools jsconfig.json (Recommended for VS Code)

🎉  Successfully created project nuxt-spa
```  
Afterwards, make sure dev mode is working
```console
$ yarn dev
```
On linux you may have to use `sudo` everytime you use `yarn`.  
Once you made sure everything is fine, close the local server (`ctrl + c`)

## 2. Setup Firebase
First let's go back to the project directory, install firebase-tools, login and init  
In my case I will add Hosting only (you can add other things later)
```console
# install firebase-tools if you haven't already
$ npm i -g firebase-tools
# login if you are not connected
$ firebase login
$ firebase init

     ######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########

You're about to initialize a Firebase project in this directory:

  C:\Users\sx0xs\Documents\Workplace\nuxt-spa

? Are you ready to proceed? Yes
? Which Firebase CLI features do you want to set up for this folder? 
Press Space to select features, then Enter to confirm your choices. 
Hosting: Configure and deploy Firebase Hosting sites

=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Please select an option: Use an existing project
? Select a default Firebase project for this directory: bary-the-app (bary-the-app)
i  Using project bary-the-app (bary-the-app)

=== Hosting Setup

Your public directory is the folder (relative to your project directory) that
will contain Hosting assets to be uploaded with firebase deploy. If you
have a build process for your assets, use your build's output directory.

? What do you want to use as your public directory? dist
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
+  Wrote dist/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

+  Firebase initialization complete!
You have now a `/functions` and a `/public` folder  
```


# Deploy
Now that we setup everything, let's focus on what we need to do everytime we want to deploy
## 1. Build  
Always build your changes before deploying  
```console
$ yarn build
```
This will build the `/dist` directory    
If you want, you can add this step in the `predeploy` script inside `hosting` in `firebase.json` like this :
```
{
  "hosting": {
    "predeploy": [
      "yarn build"
    ],
    "public": "dist",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```
This way, it will build automatically everytime you want to deploy

## 2. Deploy !
If everything went right, you now simply need to run `firebase deploy` !
```console
$ firebase deploy
```

# Conclusion 
That was fast, right ? However this doesn't use Nuxt and Firebase to their full potential (ssr -nuxt universal mode- with firebase functions). This is slightly more complicated as I'm still facing issues, but I will try to find a way to implement it.
