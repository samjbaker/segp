# RESTful API
## Week 5: 09/03/2021 (updated following reading week)

A RESTful API is an Application Programming Interface (API) that uses HTTP verbs like GET, PUT,
POST, and DELETE to operate data. We will use the principles of a REST API to start building a
backend for our site. RESTful is ideal for scaling because the state to handle the request is
contained within the request itself, in other words, the client must include all information for the
server to fulfill the request. We will be starting simple, but the principles contained here are very
powerful and can be scaled right up for whatever product you're building!

> ### Milestone checklist
> - [ ] Essential: [Workshop walkthrough](https://web.microsoftstream.com/video/e56e8487-e3b3-416f-9f9a-18498aa7bdf9)
> - [ ] Essential: [Setup and test server.js]
> - [ ] Essential: [Setup and test api.js]
***

# Express

We are building with the MEAN stack - so we will be using the Express framework for our API. There
are of course many other ways to build an API and you are free to use another toolchain should you
wish. First we need to install express and axios as a deps to our project:
'''shell npm install --save express body-parser npm install --save axios
'''

Read about axios [here](https://github.com/axios/axios).

We are building with the MEAN stack - so we will be using the Express framework for our API. There
are of course many other ways to build an API. First we need to install express and axios as a deps
to our project:

```shell
npm install --save express body-parser
npm install --save axios
```

## server.js

Then create a file server.js and a folder server in the root of our angular project. The server.js
file will have the server code, that will point to the server folder, where the rest of the server
implementation is.

```js
// Get dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const bodyParser = require('body-parser');

// Get our API routes
const api = require('./server/routes/api');

const app = express();

// Parsers for POST data
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}));

// Point static path to dist (folder where build files are located)
app.use(express.static(path.join(__dirname, 'dist/dashboard')));

// Set our api routes
app.use('/api', api);

// Catch all other routes and return the index file
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, 'dist/index.html'));
});

/**
 * Get port from environment and store in Express.
 */
const port = process.env.PORT || '3000';
app.set('port', port);

/**
 * Create HTTP server.
 */
const server = http.createServer(app);

/**
 * Listen on provided port, on all network interfaces.
 */
server.listen(port, () => console.log(`API running on localhost:${port}`));
```

## api.js

The /api route points to a file ./server/routes/api.js. Let's create this file.

```js
const express = require('express');
const router = express.Router();

/* GET api listing. */
router.get('/', (req, res) => {
  res.send('api works');
});

module.exports = router;
```

## see how it works...

Now is a good point to test that your API works. First, you need to make sure that you've run ```ng build```, which is a command to Angular CLI to ensure that the build folder (dist) is created and populated. When you previously ran ```ng serve``` this actually initiated a build and server process - which clearly won't happen now that you're serving your files using Express. Once you have a sucessful build...

Run

```shell
# to start express and take a look:
node server.js
```

> http://localhost:3000/ -> should load your site as before but this time served by Express

> http://localhost:3000/api -> should load the api and give you an 'API works' message

Now try making a curl request from your terminal;

```shell
curl http://localhost:3000/api
curl -I http://localhost:3000/api // get the headers
```

If you've got this far you have a working API. The next steps are to render the content from the API
and to call your MongoDB instance!

```shell
http://localhost:3000/ -> should load your site
http://localhost:3000/api -> should load the api
```

### Notes

Based on the following example:  
https://scotch.io/tutorials/mean-app-with-angular-2-and-the-angular-cli
