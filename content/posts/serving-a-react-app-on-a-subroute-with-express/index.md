+++
title = "Serving a React App on a Subroute With Express"
date = 2020-10-01T10:15:00-04:00
tags = ["React", "Express"]
categories = ["Software-Dev"]
draft = false
+++

There will come a time when you are looking to serve a create react app on a subroute via express. Look no further!


# Router "basename" {#router-basename}

There are a few places you need to indicate the route you will be serving the app from. Setting "basename" is the "BrowserRouter" component will make sure the router works correctly.

```js
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import { BrowserRouter } from "react-router-dom";
import * as serviceWorker from "./serviceWorker";

ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter basename="/pun/dev/hpc_2">
      <App />
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById("root")
);
```


# Package.json "homefolder" {#package-dot-json-homefolder}

```json
{
  "homepage": "https://academic-login.rc.fas.harvard.edu/pun/dev/hpc_2",
}
```

This "homepage" value is exposed in the `process.env.PUBLIC_URL` variable at build time, so anywhere that you want to use a relative url, make sure to reference it like below:

```js
useEffect(() => {
  axios
    .get(`${process.env.PUBLIC_URL}/api/sharedPartitionData`)
    .then((userSharedPartitionData) => {
      setSharedPartitionData(userSharedPartitionData.data);
    })
    .catch(function (error) {
      console.log(error);
    });
}, []);
```


# Express configuration {#express-configuration}

Now you have your react app ready to go, and you're looking to serve it using express. The strategy is to store the react app in a subdirectory to the express app. See the following tree structure, where `client` is a react app:

```nil
.
├── app.js
├── client
│   ├── package.json
│   ├── public
│   │   ├── favicon.ico
│   │   ├── favicon.png
│   │   ├── index.html
│   │   ├── logo192.png
│   │   ├── logo512.png
│   │   ├── manifest.json
│   │   └── robots.txt
│   ├── src
│   │   ├── App.css
│   │   ├── App.js
│   │   ├── App.js.backup
│   │   ├── App.test.js
│   │   ├── components
│   │   │   ├── FairshareCardGrid.jsx
│   │   │   ├── FairshareTable.jsx
│   │   │   ├── PartitionCardGrid.jsx
│   │   │   ├── StorageCardGrid.jsx
│   │   │   └── TripleBar.jsx
│   │   ├── Fairshare.jsx
│   │   ├── Hello.js
│   │   ├── index.css
│   │   ├── index.js
│   │   ├── logo.svg
│   │   ├── PrivatePartitions.jsx
│   │   ├── serviceWorker.js
│   │   ├── setupTests.js
│   │   ├── SharedPartitions.jsx
│   │   └── Storage.jsx
│   └── yarn.lock
├── package.json
├── README.md
├── yarn-error.log
└── yarn.lock
```

For the express app you just need a single `app.js` file. Set the `basename` in the `basename` variable.

```js
const express = require("express");
const bodyParser = require("body-parser");
const path = require("path");
const app = express();
const port = process.env.PORT || 5000;
const basename = "/pun/dev/hpc_2";
const apiBasename = basename + "/api";
const username = process.env.USER || "";


// expose the body portion of an incoming request stream on req.body
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// example api endpoint
app.get(apiBasename + "/user", (req, res) => {
  res.json({ username: username });
});

// Serve any static files
app.use(basename, express.static(path.join(__dirname, "client/build")));

// Handle React routing, return all requests to React app
app.get(basename + "/*", function (req, res) {
  res.sendFile(path.join(__dirname, "client/build", "index.html"));
});

app.listen(port, () => console.log(`Listening on port ${port}`));
```

You can see an example api endpoint at `` `${basename}/api/user` ``. It returns the username of the user running the express app.


# Development {#development}

During development, you could rebuild the react app and serve it with express on port 5000 by running `node app.js`, but this requires rebuilding the react app everytim you want to visualize changes to the user experience. Instead you can use a reverse proxy from the react app to the express backend to speed up development. To accomplish this, add the following to your clientside `package.json`:

```json
{
  "proxy": "http://localhost:5000",
}
```

This enables you to start the frontend by running `cd client && yarn start` and start the backend by running `node app.js`. The backend runs on port 5000. The frontend runs on port 3000 and proxies api requests to the backend.

Prepare a production build of the frontend with `cd client && yarn build`. Then you can run `node app.js` and the backend will serve the frontend.

Check out [hpc-status-app](https://github.com/Ruborcalor/hpc-status-app) for a working demo of a react app served on a subroute!

Links:

-   [Deploying a react app to a subdirectory](https://medium.com/@svinkle/how-to-deploy-a-react-app-to-a-subdirectory-f694d46427c1)
-   [Create react app deployment](https://create-react-app.dev/docs/deployment/)
