Node-Resful-Api
===============

##What is REST?
**REST** (Representational State Transfer) is an architecture style for designing networked applications. It Relies on Stateless, Client-Server, Cacheable Communicationjs Protocol and is Most often done so over HTTP.
It allows CRUD (Create, Read, Update & Delete) Operations over HTTP Requests using  `GET`, `POST`, `PUT` &, `DELETE`.
REST allows applications to retrieve a resource using a URL for example:

Yahoo's weather API allows us to get a XML Resource containing the forcast for a location,  <a href="http://weather.yahooapis.com/forecastrss?w=560743&u=c">http://weather.yahooapis.com/forecastrss?w=560743&u=c</a> allows us to get the forcast for Dublin, Ireland with Tempature in Degrees Celcius.

Resource's Do not have to be in XML, they can be in almost any format, CVS, JSON (which is what we will be using later on) or even HTML but this is not recommended unlessyou are required to return a human readable Document. If ypou would like to go into more detail such as the Key components or comparison with SOAP I recommend chcking out <a href="http://rest.elkstein.org/">this Blog</a>

I chose to persue a RESTful Service purely because of its simplicty in setting up. So without further ado....Lets Jump in.

Lets Set up a basic Node.js Project. First we need to create a folder and from inside initialise a Node app, then Install a few modules. We will be using the <a href="">Express 4.0</a> framework to do most of the heavy lifting, with a few modules to make things a little more convient.

```bash
// init project : will require some user interaction such as enter project name and version
npm init
// installs express and saves to dependiences in package.json created in step above
npm install express --save
// installs path, a module containing utilities for dealing with file paths
npm install path --save
```

Now create a `server.js` file in the main project directory and add the following

create server.js and code most basic server

```js
var express = require('express');
var path = require('path');
var app = express();

// starting static fileserver, that will watch `public` folder (in our case there will be `index.html`)
app.use(express.static(path.join(__dirname, "public")));

app.get('/api', function(request, response){
    response.send('BAM! you got a response');
});

app.listen(1234, function(){
    console.log('Our First Express/Node Server listening on port 1234');
});
```

BAM! There you have it, a very simple RESTful service.Run it with the command
```bash
node server.js
```
Now try testing with [Postman](http://www.getpostman.com/)
![Postman Screen Shot](https://raw.githubusercontent.com/jonniedarko/Node-Resful-Api/master/screenshots/Postman%20screen%20shot%201.jpg)

[Code so far](https://github.com/jonniedarko/Node-Resful-Api/commit/5b12f157674625d7d05a954c7a7e81122304c017)

Note we also went a small step further then the scope of the project with the line
```js
app.use(express.static(path.join(__dirname, "public")));
```
All this does is allows use to server static files from a public directory, so as well as creating a very basic RESTful service we also created a very basic File Server. All we need to do is create our *"public"*

Now that we can serve from a public directory we can add static content in our public directory. Give it a go, save a basic HTML Page into the public directory and point your browser to the<a href="http://localhost:1234">localhost on port 1234</a> and you sould retrieve the HTML page in the public directory.

If you want to continue with front end dev you can create your static *HTML*, *JS* and *CSS* here, you could even install  bower(npm of the front end) but we'll touch on this more in the future when we want to combine our server-side and Front end into a fullstack app. For now I won't be covering anymore front end for this post.

Back to the server-Side

We want to create a barebones CRUD api, so first we need another module, expresses *"method-override"* module, which lets us use HTTP verbs such as PUT or DELETE in places where the client doesn't support it

_**NOTE:** It is very important that this module is used before any module that needs to know the method of the request, otherwise it could cause issues which may be difficult to track down._

So lets install it:
```bash
    npm install method-override --save
```
Now inside server.js add the following just after the express require
```js
//add after our express require
var methodOverride = require('method-override');
//add after the static directory setup
app.use(methodOverride());
```

No we can simulate DELETE and PUT, so lets add some Endpoints to our REST Api. Add the following just before the `app.listen(...)`

```js
app.get('/api/user', function(request, response) {
    response.send('This is not implemented now');
});

app.post('/api/user', function(request, response) {
    response.send('This is not implemented now');
});

app.get('/api/user/:id', function(request, response) {
    response.send('This is not implemented now');
});

app.put('/api/user/:id', function (request, response){
    response.send('This is not implemented now');
});

app.delete('/api/user/:id', function (request, response){
    response.send('This is not implemented now');
});
```

What we have done here is create a Skeleton for:
1 - `GET`ting all users @ `/api/user`
2 - `POST`ing or Persisting/Saving a new user to same URL
3 - `GET`ting a user of a particular `id` @ `/api/user/:id` e.g. `/api/user/12345`
4 - `PUT`ting or updating a particular user @ the same URL
5 - `DELETE` a particular User

BUT.....before we can take the next step with our restfull api we need to set up where and how our data is stored. This is where [MongoDB](http://www.mongodb.org/) & [Mongoose.js](http://mongoosejs.com) come into play..

Firstly make sure you have MongoDB installed and running (Installers are available[here](http://www.mongodb.org/downloads), I Personally used [Homebrew to install](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/)). Once installed you can usually start by running the command `mongod`.

Next install the Node Mongodb module as well as the Mongoose.js module
```bash
npm install mongoose --save
```
Rather then make our Server overly complex and confusing then it needs to be, lets create a CRUD module to store our database interaction. Create two new folders, called lib and config. Inside config create a new javascript file *"db.config.js"* to hold our DB configuration.

So lets get a connection to mongodb going...

```js
module.exports = function () {
    var mongoose = require('mongoose');

    mongoose.connect('mongodb://localhost:27017/crudusers');

    var db = mongoose.connection;

    db.on('error', function(error){
        console.log("connection error: "+error.message);
    });

    db.once('open', function(){
        console.log("Connected successfull!")
    });
}
```

And *require* our configuration in our *"server.js"* file.
```js
// Add the following after requiring the "path" module
require("./config/db.config")();
```

Now if we run `node server.js` you should get the following output:

```bash
Our First Express/Node Server listening on port 1234
Connected successfull!
```

If you are getting `connection error: failed to connect to [localhost:27017]` then you most likely forgot to start Mongodb.

Next we want to create a Schema and from that a Model which we can export to use in our serve routes. So inside our lib folder create *"user.model.js"* and add the following:

```js
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
var User = new Schema({
    firstname: { type: String, required: true},
    surname:  { type: String, required: true},
    username: { type: String, required: true},
    password: { type: String, required: true}
});
var UserModel = mongoose.model("User", User);

module.exports.UserModel = UserModel;
```

In order to use this model we need to import the module but before we doe this ltest clean up our code base by moving our routes from *"server.js"* to a new file. Create a new file called *"rest.routes.js"* inside the *"lib"* folder. What we are going to do is return a function that accepts our Express "app" as a parameter.

```js
// rest.routes.js
module.exports = function(app){
    app.get('/api', function(request, response){
        response.send('BAM! you got a response');
    });

    app.get('/api/user', function(request, response) {
        response.send('This is not implemented now');
    });

    app.post('/api/user', function(request, response) {
        response.send('This is not implemented now');
    });

    app.get('/api/user/:id', function(request, response) {
        response.send('This is not implemented now');
    });

    app.put('/api/user/:id', function (request, response){
        response.send('This is not implemented now');
    });

    app.delete('/api/user/:id', function (request, response){
        response.send('This is not implemented now');
    });
}

```

And our *"server.js"* should look like the following

```js
var express = require('express');
var methodOverride = require('method-override');
var path = require('path');
var restRoutes = require('./lib/rest.routes.js');
require('./config/db.config')();

var app = express();
app.use(express.static(path.join(__dirname, "public")));
app.use(methodOverride());

// Include our routes from "rest.routes.js"
restRoutes(app);

app.listen(1234, function(){
    console.log('Our First Express/Node Server listening on port 1234');
});
```

Now Lets make use of our Model by importing into our rest routes module.

```js
var UserModel    = require('./user.model').UserModel;
```

And now we update our routes, lets start with the `POST` method so we can actually create data. Replace out post method with the following

```js
app.post('/api/user', function (request, response) {
   var user = new UserModel({
        firstname: request.body.firstname,
        surname:  request.body.surname,
        username: request.body.username,
        password: request.body.password
   });
   user.save( function (error){
        if(!error){
            console.log("User '"+user.username+"' Created!");
            return response.send({status: 'OK', user:user})
        } else {
            console.log(err);
            response.statusCode = 500;
            response.send({ error: 'Server error' });
        }
   });
});
```

Now if you try to post to this you may run into a few issues like "firstname not defined" and if you debug it you will probably find the same is true for request.body....

This is because we need to include the express [body-parser](https://github.com/expressjs/body-parser), in Express 4 we do this by requiring it and including the following with the rest of our configuration:

Install

```bash
npm install body-parser --save
```

and include & use in our *"server.js"*

```js
var bodyParser = require('body-parser');

......

app.use(bodyParser.urlencoded({extended: true}))
```

this allows us to parse application/x-www-form-urlencoded paramaters, the extended true allows us to extended parse syntax with the [qs module](https://github.com/visionmedia/node-querystring)

In postman we setup a request up as shown below
![POSTing a new User](https://raw.githubusercontent.com/jonniedarko/Node-Resful-Api/master/screenshots/Postman%20screen%20shot%202.jpg)

[Code so far](https://github.com/jonniedarko/Node-Resful-Api/commit/364bb87393fa0bd4bb16cd1e139259506336b197)

Now lets set up our routes to get all users, update a user and delete a user:

```js
app.get('/api/user', function (request, response) {
    return UserModel.find(function (error, users) {
        if (!error) {
            return response.send(users);
        } else {
            response.statusCode = 500;
            console.log('Internal error(%d): %s',response.statusCode,error.message);
            return response.send({ error: 'Server error' });
        }
    });
});

app.get('/api/user/:id', function (request, response) {
    return UserModel.findById(request.params.id, function (error, user) {
        if(!user) {
            response.statusCode = 404;
            return response.send({ error: 'Not found' });
        }
        if (!error) {
            return response.send({ status: 'OK', user:user });
        } else {
            response.statusCode = 500;
            console.log('Internal error(%d): %s',response.statusCode,error.message);
            return response.send({ error: 'Server error' });
        }
    });
});
app.put('/api/user/:id', function (request, response){
    return UserModel.findById(request.params.id, function (error, user) {
        if(!user) {
            response.statusCode = 404;
            return response.send({ error: 'Not found' });
        }

        user.firstname = request.body.firstname;
        user.surname = request.body.surname;
        user.username = request.body.username;
        user.password = request.body.password;
        return user.save(function (error) {
            if (!error) {
                console.log("user updated");
                return response.send({ status: 'OK', user:user });
            } else {
                if(error.name == 'ValidationError') {
                    response.statusCode = 400;
                    response.send({ error: 'Validation error' });
                } else {
                    response.statusCode = 500;
                    response.send({ error: 'Server error' });
                }
                console.log('Internal error(%d): %s',response.statusCode,error.message);
            }
        });
    });
});

app.delete('/api/user/:id', function (request, response){
    return UserModel.findById(request.params.id, function (error, user) {
        if(!user) {
            response.statusCode = 404;
            return response.send({ error: 'Not found' });
        }
        return user.remove(function (err) {
            if (!error) {
                console.log("User removed");
                return response.send({ status: 'OK' });
            } else {
                response.statusCode = 500;
                console.log('Internal error(%d): %s',response.statusCode,error.message);
                return response.send({ error: 'Server error' });
            }
        });
    });
});
```

Just to be Sure test with postman to verify all behave as expected, you can even use (Robomongo)[http://robomongo.org/] to see what data is in the Database.

And there you go, your first restfull API... its pretty simple but also lacking a few important features. In the Next tutorial we're goin to introduce Mongoose validation to ensure values being added to our database are as expected, encription on our password field.

<div style="display:none">
Sources
- http://aleksandrov.ws/2013/09/12/restful-api-with-nodejs-plus-mongodb/
- http://scotch.io/bar-talk/expressjs-4-0-new-features-and-upgrading-from-3-0
- https://github.com/expressjs/method-override
- http://thewayofcode.wordpress.com/2013/04/21/how-to-build-and-test-rest-api-with-nodejs-express-mocha/
</div>

