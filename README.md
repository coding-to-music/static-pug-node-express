# static-pug-node-express

https://static-pug-node-express.herokuapp.com

https://github.com/coding-to-music/static-pug-node-express

Treehouse Project 7 - React Image Gallery App

By JEN GERMANN germanny https://github.com/germanny

https://jengermann-portfolio.herokuapp.com

https://github.com/germanny/static-node-and-express-site

```
GitHub:
```

### Flickr API Keys

Create a file - `src/data/config.js` - with the contents:

```
export const keys = {
  apiKey: '[KEY]',
  apiSecret: '[SECRET]'
};

```

# static-pug-node-express

<div id="top"></div>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <!-- <a href="https://github.com/davidcastillog/travelfy-server">
    <img src="https://github.com/davidcastillog/travelfy-client/blob/master/src/assets/images/travelfy-logo-blue.jpg" alt="travelfy Logo" width="250" height="75" alt="Travelfy">
  </a> -->
  <h3 align="center">static-pug-node-express</h3>
     Pug React Axios stack app
    <br />
   <a href="https://static-pug-node-express.herokuapp.com">View Demo</a>
</div>

# static-pug-node-express

https://github.com/coding-to-music/static-pug-node-express

https://static-pug-node-express.herokuapp.com/

```java
cors({
  credentials: true,
  origin: [
    "https://static-pug-node-express.herokuapp.com",  # Heroku Production
    "https://still-sands-27981.herokuapp.com",              # test
    "https://guarded-dusk-08830.herokuapp.com",             # staging
    "https://radiant-gorge-70504.herokuapp.com",            # development
    "http://localhost:3000",
```

## Display Build Date and Environment

https://stackoverflow.com/questions/53028778/how-to-show-build-datetime-on-my-react-web-app-using-create-react-app

Starting in Create React App 2 (react-scripts@^2.0) you can accomplish this via macros.

First, install preval.macro:

```java
$ npm install --save preval.macro # if using npm
$ yarn add preval.macro # if using yarn
```

Next, in the file you want to render a build timestamp in, include preval.macro:

```java
import preval from 'preval.macro'
```

Finally, you can create a constant and use it in your app like so:

```java
const dateTimeStamp = preval`module.exports = new Date().toLocaleString();`
```

```java
const AppEnvironment = process.env.REACT_APP_ENVIRONMENT || "unknown";

console.log(`App.js: AppEnvironment: ${AppEnvironment}`);
```

Set the environment variable ENVIRONMENT to one of the following:

```java
heroku config:set REACT_APP_ENVIRONMENT="production"
```

Here's a full example:

```java
import React, { Component } from 'react'
import logo from './logo.svg'
import './App.css'
import preval from 'preval.macro'

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
          <p>
            Build Date: {preval`module.exports = new Date().toLocaleString();`}.
          </p>
          <p>AppEnvironment: {AppEnvironment}</p>
          </p>
        </header>
      </div>
    )
  }
}

export default App
```

## The Problem - Displaying the Build Date

I want to show the current git branch and commit hash in my (Create) React App.

Others have done this by setting custom environment variables, like `REACT_APP_GIT_BRANCH` and `REACT_APP_GIT_COMMIT_HASH`. While this works okay on macOS and Linux, it doesn't play nice with Windows. (Windows can _read_ the environment variables fine, it's the _setting_ them that is painful).

## A Solution

The following solution works on macOS, Linux, and Windows.

In a nutshell, it:

- Runs a small NodeJS script
- This script uses the `git` CLI to grab the current git branch and commit hash, then writes that info to a JSON file
- This JSON file is then available at build-time, so the React App can import it

### `package.json`

The `git-info` script is new. It must be added as a pre-command to the existing scripts.

```json
{
  "scripts": {
    "git-info": "node src/gitInfo.js",
    "start": "npm run git-info && react-scripts start",
    "build": "npm run git-info && react-scripts build",
    "test": "npm run git-info && react-scripts test"
  }
}
```

### `src/gitInfo.js`

NodeJS lets you execute shell commands with `child_process.execSync`. We wrap it with `execSyncWrapper` to clean things up. If there's an issue running a `git` command, the JSON file is still written, and the value is just `null`.

```js
const fs = require("fs");
const path = require("path");
const { execSync } = require("child_process");

const execSyncWrapper = (command) => {
  let stdout = null;
  try {
    stdout = execSync(command).toString().trim();
  } catch (error) {
    console.error(error);
  }
  return stdout;
};

const main = () => {
  let gitBranch = execSyncWrapper("git rev-parse --abbrev-ref HEAD");
  let gitCommitHash = execSyncWrapper("git rev-parse --short=7 HEAD");

  const obj = {
    gitBranch,
    gitCommitHash,
  };

  const filePath = path.resolve("src", "generatedGitInfo.json");
  const fileContents = JSON.stringify(obj, null, 2);

  fs.writeFileSync(filePath, fileContents);
  console.log(`Wrote the following contents to ${filePath}\n${fileContents}`);
};

main();
```

### `src/generatedGitInfo.json`

This file is generated by the script. So, it should be added to your `.gitignore` file.

```json
{
  "gitBranch": "main",
  "gitCommitHash": "352e1c7"
}
```

### `src/App.js`

Finally, we can display the info in our app. Just import the JSON file, and reference it in your JSX.

```jsx
import logo from "./logo.svg";
import "./App.css";
import generatedGitInfo from "./generatedGitInfo.json";

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <div className="git-info">
          <p>
            <strong>Git Branch:</strong>{" "}
            <code>{generatedGitInfo.gitBranch}</code>
          </p>
          <p>
            <strong>Git Commit Hash:</strong>{" "}
            <code>{generatedGitInfo.gitCommitHash}</code>
          </p>
        </div>
      </header>
    </div>
  );
}

export default App;
```

<!-- TABLE OF CONTENTS -->
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#api">API required</a></li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>

<!-- ABOUT THE PROJECT -->

## Tasks To Do

- [&check; ] Enviroments for development, test, staging, production
- [x] Display Enviroment in console
- [✅] Checkmarks Working
- [ ] User error in console
- [✅] Display Build and Environment info in the UI
- [ ] Get Login to work
- [ ] Logging
- [ ] Auth with GitHub and Google
- [ ] Farenheit default
- [ ] Detect user's location and use correct Farenheit vs Celsius
- [ ] Dark mode
- [ ] Dark maps
- [ ] News API
- [ ] Stocks API
- [ ] Embedded Charts
- [ ] Detect Dark vs Light preference
- [ ] Display Location Info
- [ ]

More Checkmark Examples

- [&check;] - html checkbox example
- [:white_check_mark:] - emoji checkbox example
- [&#9746;] - unicode checkbox example
- [✅] - unicode checkbox example

## About The Project

<p align="right"><a href="#top">back to top ^</a></p>

### Built With

- [React.js](https://reactjs.org/)
- [Material-UI](https://mui.com/)
- [Node.js](https://nodejs.org/en/)
- [Express.js](https://expressjs.com/)
- [MongoDB](https://www.mongodb.com/)

<p align="right"><a href="#top">back to top ^</a></p>

<!-- GETTING STARTED -->

## Getting Started

To get a local copy up and running follow these simple example steps.

### Prerequisites

Verify if you already have Node.js and npm installed:

```sh
node -v
npm -v
```

To download the latest version of npm, on the command line, run the following command:

```sh
npm install -g npm
```

<p align="right"><a href="#top">back to top ^</a></p>

<!-- API LIST -->

<p align="right"><a href="#top">back to top ^</a></p>

<!-- SCRIPTS -->

## Scripts

In the project directory:

```sh
 npm run dev
```

Runs the app in the development mode.\
Open [http://localhost:5005](http://localhost:5005)

You may also see any lint errors in the console.

<p align="right"><a href="#top">back to top ^</a></p>

<!-- USAGE EXAMPLES -->

<!-- MARKDOWN LINKS & IMAGES -->

## Installation:

### GitHub

```java
git init
git add .
git remote remove origin
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:coding-to-music/static-pug-node-express.git
git push -u origin main
```

## Need to set CORS in server/config/index.js

```
  // controls a very specific header to pass headers from the frontend
  app.use(
    cors({
      credentials: true,
      origin: [
        "https://static-pug-node-express.herokuapp.com",  # Heroku Production
        "https://still-sands-27981.herokuapp.com",              # test
        "https://guarded-dusk-08830.herokuapp.com",             # staging
        "https://radiant-gorge-70504.herokuapp.com",            # development
        "http://localhost:3000",
        "*",
      ],
    })
  );
```

<!-- Heroku Login -->

### Heroku Login

## Just adding some detailed steps to resolve the issue

- Navigate to https://dashboard.heroku.com/account/applications
- In Authorizations click on create authorization button
- Add description in pop up eg.heroku cli and leave expire after blank if you dont want it to expire
- You will get authorization token
- in cli run heroku login -i
- when it prompts for password enter the authoriation token

### Heroku

```java
heroku create static-pug-node-express
```

```java
heroku create --remote development
Creating app... done, ⬢ radiant-gorge-70504
https://radiant-gorge-70504.herokuapp.com/ | https://git.heroku.com/radiant-gorge-70504.git
```

```java
heroku create --remote staging
Creating app... done, ⬢ guarded-dusk-08830
https://guarded-dusk-08830.herokuapp.com/ | https://git.heroku.com/guarded-dusk-08830.git
```

```java
heroku create --remote test
Creating app... done, ⬢ still-sands-27981
https://still-sands-27981.herokuapp.com/ | https://git.heroku.com/still-sands-27981.git
```

### Heroku MongoDB Environment Variables

```java
heroku config:set MONGODB_URI="mongodb+srv://<userid>:<password>@cluster0.zadqe.mongodb.net/static-pug-node-express?retryWrites=true&w=majority"
git push heroku
```

### Push to Heroku

```
git push heroku
```

## Installation:

## Procfile for however to start the Client

Procfile

```java
web: npm run start:client
```

## Modify server/index.js

```java
// dotenv config
require("dotenv").config();

// app and port config
const app = express();
const port = process.env.PORT || 4000;

// Priority serve any static files.
app.use(express.static(path.resolve(__dirname, "../react-ui/build")));

// Answer API requests.
app.get("/api", function (req, res) {
  res.set("Content-Type", "application/json");
  res.send('{"message":"Hello from the custom server!"}');
});

// All remaining requests return the React app, so it can handle routing.
app.get("*", function (request, response) {
  response.sendFile(path.resolve(__dirname, "../react-ui/build", "index.html"));
});
```

### Project Directory Structure

```java
tree -d -L 2
.
├── client
│   ├── build
│   ├── node_modules
│   ├── public
│   └── src
├── node_modules
└── server
    ├── controllers
    ├── database
    ├── models
    └── routes
```

### Package.json

key items in package.json

- cacheDirectories for Heroku to save time on builds

- engine set the node version to use

main scripts

`npm run` and any of the scripts below:

- install
- build
- start
- deploy

```java
{
  "name": "static-pug-node-express-server",
  "version": "1.0.0",
  "description": "MongoDB Dev MERN Stack",
  "main": "index.js",
  "engines": {
    "node": "16.x"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server",
    "build": "cd client/ && npm install && npm run build",
    "watch": "nodemon server",
    "deploy": "git add . && git commit -m Heroku && git push && git push heroku",
    "deploy:staging": "git push staging main",
    "deploy:test": "git push test main",
    "deploy:development": "git push development main && heroku logs --tail --remote development",
    "install:client": "cd client && npm install --production=false",
    "build:client": "cd client && npm run build",
    "install:server": "npm install --production=false",
    "install:server:client": "concurrently \"npm install --production=false\" \"npm install:client\"  \"npm install:server\"",
    "start:client": "cd client && npm run start",
    "start:server": "npm run start",
    "start:all": "concurrently \"npm run start:client\" \"npm run start:server\""
  },
  "keywords": [
    "mern"
  ],
  },
  "cacheDirectories": [
    "node_modules",
    "client/node_modules"
  ]
}
```

## scripts for staging and test

```java
    "deploy:staging": "git push staging main",
    "deploy:test": "git push test main",
```

## Testing

```java
npm run deploy:test

heroku logs --tail --remote test
```

```java
npm run deploy:test

heroku logs --tail --remote staging
```

## Specify the app you want with --app or --remote.

heroku apps

```java
list of apps
```

# Heroku remotes in repo:

```java
static-pug-node-express (heroku)
guarded-dusk-08830 (staging)
still-sands-27981 (test)
```

## git remote -v

git remote -v

```java
heroku  https://git.heroku.com/react-gallery-portfolio.git (fetch)
heroku  https://git.heroku.com/react-gallery-portfolio.git (push)
origin  git@github.com:coding-to-music/static-pug-node-express.git (fetch)
origin  git@github.com:coding-to-music/static-pug-node-express.git (push)
```

## next steps

- Change the git remote for heroku FROM `react-gallery-portfolio`
- push to the heroku remote TO `static-pug-node-express`

- redeploy the heroku app

### change remote

```java
git remote remove heroku
git remote add heroku https://git.heroku.com/static-pug-node-express.git
```

### Verify is correct:

git remote -v

```java
heroku  https://git.heroku.com/static-pug-node-express.git (fetch)
heroku  https://git.heroku.com/static-pug-node-express.git (push)
origin  git@github.com:coding-to-music/static-pug-node-express.git (fetch)
origin  git@github.com:coding-to-music/static-pug-node-express.git (push)
```

## PRODUCTION

```java
heroku config:set REACT_APP_ENVIRONMENT="production" --remote heroku
```

## STAGING

```java
heroku config:set REACT_APP_ENVIRONMENT="staging" --remote staging
```

## TEST

```java
heroku config:set REACT_APP_ENVIRONMENT="test" --remote test
```

## DEVELOPMENT

```java
# Another-Dark 8f74bcd4b356ed24
REACT_APP_GOOGLE_MAP_ID="8f74bcd4b356ed24"
heroku config:set REACT_APP_ENVIRONMENT="development" --remote development
heroku config:set REACT_APP_GOOGLE_MAP_ID="8f74bcd4b356ed24" --remote development
# https://dashboard.heroku.com/apps/still-sands-27981
# https://still-sands-27981.herokuapp.com/
```

### GitHub

```java
nvm use v16
npx browserslist@latest --update-db
git init
git add .
git remote remove origin
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:coding-to-music/static-pug-node-express.git
git push -u origin main
```

### Heroku

```java
heroku create static-pug-node-express

heroku run npx browserslist@latest --update-db
```

### .env MongoDB Environment Variables

```java
ATLAS_URI="mongodb+srv://<userid>:<password>@cluster0.zadqe.mongodb.net/static-pug-node-express?retryWrites=true&w=majority"
```

### Heroku MongoDB Environment Variables

```java
heroku config:set ATLAS_URI="mongodb+srv://<userid>:<password>@cluster0.zadqe.mongodb.net/static-pug-node-express?retryWrites=true&w=majority"
```

Set all the environment variables on Heroku

```java
heroku config:set MONGODB_URI="mongodb+srv://<userid>:<password>@cluster0.zadqe.mongodb.net/static-pug-node-express?retryWrites=true&w=majority"

git push heroku
```

### Heroku Buildpack

See this repo for more info about setting up a node/react app on heroku:

https://github.com/mars/heroku-cra-node

```java
heroku buildpacks

heroku buildpacks --help

heroku buildpacks:clear

```

### Notice we are doing a SET and then and ADD

```java
heroku buildpacks:set heroku/nodejs

heroku buildpacks:add mars/create-react-app

```

Output:

```java
Buildpack added. Next release on static-pug-node-express will use:
  1. heroku/nodejs
  2. mars/create-react-app
Run git push heroku main to create a new release using these buildpacks.
```

### Lets try reversing the order

```java
heroku buildpacks:set mars/create-react-app

heroku buildpacks:add heroku/nodejs
```

```java
heroku buildpacks
```

Output:

```java
=== static-pug-node-express Buildpack URLs
1. mars/create-react-app
2. heroku/nodejs
```

### Push to Heroku

```
git push heroku
```

## Error:

```java
2022-03-29T08:06:58.791397+00:00 heroku[web.1]: Starting process with command `npm start`
2022-03-29T08:06:59.934025+00:00 app[web.1]: ls: cannot access '/client/build/static/js/*.js': No such file or directory
2022-03-29T08:06:59.938326+00:00 app[web.1]: Error injecting runtime env: bundle not found '/client/build/static/js/*.js'. See: https://github.com/mars/create-react-app-buildpack/blob/master/README.md#user-content-custom-bundle-location
```

Use this to fix:

```java

heroku config:set JS_RUNTIME_TARGET_BUNDLE=./react-ui/build/static/js/*.js

heroku run npx browserslist@latest --update-db

heroku run npm i tree && tree -d -L 2

```

## Local Development

Because this app is made of two npm projects, there are two places to run `npm` commands:

1. **Node API server** at the root `./`
1. **React UI** in `react-ui/` directory.

### Run the API server

In a terminal:

```bash
# Initial setup
npm install

# Start the server
npm start
```

#### Install new npm packages for Node

```bash
npm install package-name --save
```

### Run the React UI

The React app is configured to proxy backend requests to the local Node server. (See [`"proxy"` config](react-ui/package.json))

In a separate terminal from the API server, start the UI:

```bash
# Always change directory, first
cd react-ui/

# Initial setup
npm install

# Start the server
npm start
```

#### Install new npm packages for React UI

```bash
# Always change directory, first
cd react-ui/

npm install package-name --save
```
