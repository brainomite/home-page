---
layout: post
title: How to Auto reload a full stack JavaScript project using nodemon and webpack dev server
# description: >
#   Howdy! This is an example blog post that shows several types of HTML content supported in this theme.
sitemap: true
---

A couple of weeks ago, when I was under the pressure of time, I decided to attempt to get webpack-dev-server (WDS) running. I failed miserably, and moved on instead of wasting time on the project. Now again, I find myself in need of a tool to speed up development time. Last week I did the research and today I’m sharing for posterity’s sake. Additionally at the time of this article [webpack-livereload-plugin is not compatible](https://github.com/statianzo/webpack-livereload-plugin/issues/40) with webpack 4 that was [just released](https://medium.com/webpack/webpack-4-released-today-6cdb994702d4) a few days ago. The only downside to this configuration is I cannot use socket.io to communicate between nodemon’s server and WDS’s client.

I’m in the middle of a rapid development of a full-stack javascript single page application (SPA). I decided that having a backend refresh automatically with each code save using nodemon would be ideal so I could implement a RESTful API. Additionally I wanted my bundle to be rebuilt and my browser to also refresh when the bundle is ready. For those two things combined, WDS will achieve my goals.

Here are the changes I had to make to my webpack.config.js & package.json file. At the end of the article I have my entire final webpack.config.js file in one shot, should you wish to copy it.
First install necessary packages if you don’t already have them using npm or yarn

```
npm install --save-dev nodemon webpack-dev-server webpack-cli
```

Before I started my output property was set to as follows:

```js
output: {
  path: __dirname,
  filename: './public/bundle.js',
},
```

The problem with this is that when WDS runs, it’ll make the bundle available at http://yourUrl:yourPort/public/bundle.js to fix this, I required ‘path’ and changed my setting to the following:

```js
output: {
  path: path.join(__dirname, '/public'),
  filename: 'bundle.js'
},
```

Next are a series of settings that go within an object at the property ‘devServer’ first up are two commands related to my static folder path. The first is the path of static content that’ll be served up at the root of the site. Second is a flag indicating whether to auto refresh if the static content changes, it is defaulted to false.

```js
devServer: {
  contentBase: path.join(__dirname, '/public'), //serve your static files from here
  watchContentBase: true,
}
```

Next up, because I want nodemon to host my api routes, I needed to set up a proxy. This basically tells WDS to play man-in-the-middle and and redirect your request to the nodemon instance. It is important that your nodemon instance and your WDS do NOT share the same ports, that will cause an error.

```js
proxy: [ // allows redirect of requests to webpack-dev-server to another destination
  {
    context: ['/api', '/auth'],  // can have multiple
    target: 'http://localhost:8080', //server and port to redirect to
    secure: false //don't use https
  }
],
```

Last important setting is the port, this allows you to override WDS’s default port of 8080 should you want. Additionally I found these interesting settings that will allow me to know if I have a build warning or error without having to stare at the console.

```js
port: 3000, // port webpack-dev-server listens to, defaults to 8080
overlay: { // Shows a full-screen overlay in the browser when there are compiler errors or warnings
  warnings: true, // default false
  errors: true, //default false
},
```

Last but not least I added the following scripts to my package.json. The last 3 scripts just open the main page using the specified browser. The — hot option enables Hot Module Replacement. One extra tidbit, when WDS bundles up the JS files, it serves it from memory. Don’t worry if you don’t observe a bundle.js in your file system after triggering these scripts.

```json
"test": "nodemon ./server/index.js",
"WDS": "npm test & webpack-dev-server --inline --hot",
"WDSC": "npm test & webpack-dev-server --inline --hot --open 'Google Chrome'",
"WDSF": "npm test & webpack-dev-server --inline --hot --open 'Firefox'",
"WDSS": "npm test & webpack-dev-server --inline --hot --open 'Safari'"
```

Finally here is my completed webpack.config.js:

```js
const path = require("path");
const isDev = process.env.NODE_ENV === "development";

module.exports = {
  mode: isDev ? "development" : "production",
  entry: [
    "@babel/polyfill", // enables async-await
    "./client/index.js",
  ],
  output: {
    path: path.join(__dirname, "/public"),
    filename: "bundle.js", // relative to the outputPath (defaults to / or root of the site)
  },
  devtool: "source-map",
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: "babel-loader",
      },
    ],
  },
  devServer: {
    contentBase: path.join(__dirname, "/public"), // serve your static files from here
    watchContentBase: true, // initiate a page refresh if static content changes
    proxy: [
      // allows redirect of requests to webpack-dev-server to another destination
      {
        context: ["/api", "/auth"], // can have multiple
        target: "http://localhost:8080", // server and port to redirect to
        secure: false,
      },
    ],
    port: 3030, // port webpack-dev-server listens to, defaults to 8080
    overlay: {
      // Shows a full-screen overlay in the browser when there are compiler errors or warnings
      warnings: false, // defaults to false
      errors: false, // defaults to false
    },
  },
};
```

This was cross-posted on [Medium](https://itnext.io/auto-reload-a-full-stack-javascript-project-using-nodemon-and-webpack-dev-server-together-a636b271c4e)
