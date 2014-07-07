Creating a Restful Service to interact with a Raspberry Pi's GPIO
===================================================================

In a Pervious series of [Posts](http://blog.jonnie.io/creating-a-restful-api-with-node-js/) I have shown how to get a Restful service up and running on Node. Today we are goin to take this a step further by using a Restful service to interact with our GPIO. For today we will simpley start with turning a light on an off.

Before we get started, Node needs to be installed on you Pi. There are many existing tutorials online but I found a post on [josh on design](http://joshondesign.com/2013/10/23/noderpi) to be the clearest and most successful, I had issues following other peoples instructions. We do not need MongoDB for this tutorial as we will not be storing any results. If you want to store Information in MongoDB then you will need to Build it yourself which is very time consuming.

From this point on I will be assuming you have Node working successfully and I will not be goin inot too much detail on the Restful Service as it is cover in pervious posts so lets just Through together a quick structure. Create a Node project as we have before:

```bash
npm init
npm install express --save
```

Then create our *"server.js"* 

```js
var express = require('express');
var app = express();

app.get('/pi', function(request, response){
    response.send('BAM! you got a response');
});

app.listen(1234, function(){
    console.log('Our First Express/Node Server listening on port 1234');
});
```

This is nothing new, infact its almost indentical to the server in a [pervious post](http://blog.jonnie.io/creating-a-restful-api-with-node-js/)

[gpio node module](https://github.com/EnotionZ/GpiO)
