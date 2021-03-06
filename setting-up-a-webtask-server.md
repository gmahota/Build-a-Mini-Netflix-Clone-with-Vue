# Setting Up a Webtask Server

First, set up a back-end server for hosting and provisioning the data for the Vue app \(the clone\).

[Webtask](https://webtask.io/) by [Auth0](https://auth0.com/) is a Function as a Service \(FaaS\) platform that empowers you to provision a hosted function as an endpoint. In a nutshell, you install the command-line interface \(CLI\) for deploying functions.

## Install Webtask

You can manage server functions either on the webtask console or through CLI. Because you are likely familiar with the CLI, you might opt for it as a more productive choice.

Run the following command to install the CLI with `npm`:

```bash
npm install -g wt-cli
```

To actually deploy functions, create a webtask account by running the following command:

```bash
wt init <YOUR-EMAIL>
```

## Test-Drive Webtask

Want to see how webtask works? Create a folder named `demo-server` and add to it an `index.js` file with the following content:

```javascript
module.exports = function (cb) {
    cb(null, 'Hello World');
}
```

Once you have run the above function with webtask, the content passes as the second argument to `cb`, which is then displayed in the browser. Run this command:

```bash
wt create index
```

Now go to a browser and open the URL that is displayed in the terminal. You see the `Hello World` string right there:

![](https://res.cloudinary.com/christekh/image/upload/v1521564028/T0RwEO5LQWu15i3UFjKu_Screen_20Shot_202017-04-26_20at_201.27.43_20PM_ngvppk.png)

## Understand the Server Dependencies

Another cool feature of webtask is that a `package.json` file can specify dependencies. Create such a file with this content:

```javascript
{
    "name": "server",
    "version": "1.0.0",
    "main": "index.js",
    "license": "MIT",
    "dependencies": {
    "body-parser": "^1.18.2",
    "express": "^4.16.3",
    "mongoose": "^5.0.10",
    "webtask-tools": "^3.2.0"
    }
}
```

Note the dependencies:

* `express` performs routing tasks, just as a regular Web server does.
* `mongoose` is an object-relational mapping \(ORM\) that works with a Mongo database you will create later.
* `body-parser` parses the HTTP body from a client.
* The [programming model](https://webtask.io/docs/model) for webtask uses functions as endpoints. `webtask-tools` is a library with which you can alter the model, creating express routes rather than functions.

Replace the `index.js` file content with the following:

```javascript
var Express = require('express');
var Webtask = require('webtask-tools');
var bodyParser = require('body-parser');
var app = Express();
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.get('/', (req, res) => {
    res.send('Hello World');
})

module.exports = Webtask.fromExpress(app);
```

The above code also prints the same content as that in the first example. However, you can set up more routes this time.

