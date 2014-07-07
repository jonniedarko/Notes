Node Restful Api Part 2 - Validation and Securing Passwords
=============================================
This Post Continues on from the [pervious post](http://blog.jonnie.io/creating-a-restful-api-with-node-js/) with the code continuing on from [this commit](https://github.com/jonniedarko/Node-Resful-Api/commit/1abb749e572aaf2536c942a43026fb2acc9c7e28). We currently have a simple RESTful service to create, delete and update users, but right now we are not enforcing any rules as to what can be submitted. One of the most important steps in making any service secure and trustworthy is Validation. This could mean making sure usernames are unique, only contain certain characters or passwords are long enough and not a regular insecure password like  "password" or "test".

There are a number of ways to enforce rules. Some require the developer to manually create rules, others are built in and only require activation. 

Lets start with the built in options in mongoose. We can make our username field unique by adding that option to our schema. So lets update our Schema by adding `index: {unique: true, dropDups: true}}` to our options.

```js
var User = new Schema({
    firstname: { type: String, required: true},
    surname:  { type: String, required: true},
    username: { type: String, required: true, index: {unique: true, dropDups: true}},
    password: { type: String, required: true}
});

```

Whats happening here is as well as enforcing a unique rule we are telling MongoDB to drop any dublicates. This is required for the unique rule to work **IF** you already have duplicates but beware, it does exactly what you think it does...it drops the records.

If you don't have any duplicates then `index: {unique: true}` will work on its own.

Next we are goin to create som Mongoose validation on our schemas. Before we do this lets create some user validation helper methods. For this lets create a seperate module to house our validation rule. Inside lib create `user.validation.js`. What we want to do is create an Object consisting of our Validation rules.

```js
module.exports = {
    isAlphaNumericOnly : function (input)
    {
        var letterNumberRegex = /^[0-9a-zA-Z]+$/;
        if(input.match(letterNumberRegex))
        {
            return true;
        }
        return false;
    },
    isLongEnough : function (input){
        if(input.length >= 6){
            return true;
        }
        return false;
    },
    isGoodPassword : function (input)
    {
        // at least one number, one lowercase and one uppercase letter
        // at least six characters
        var regex = /(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{6,}/;
        return regex.test(input);
    }    
}
```

Each of these methods takes a string as an input and preforms a validation test on it:
* __isAlphaNumericOnly__: uses a regular Expression that returns true if the input only contains Alpha or numerical characters.
* __isLongEnough__: returns true if input is at least 6 characters long
* __isGoodPassword__: returns true if the input contains at least one number, one lowercase and one uppercase letter and is at least six characters long 

What about SQL injection?
>As a client program assembles a query in MongoDB, it builds a BSON object, not a string. Thus traditional SQL injection attacks are not a problem.

Dollar Sign Operator Escaping
>Field names in MongoDB’s query language have semantic meaning. The dollar sign (i.e $) is a reserved character used to represent operators (i.e. $inc.) Thus, you should ensure that your application’s users cannot inject operators into their inputs.

So While we may not need to worry about SQL injection, we do need to consider the `$` issue. There are many ways to approach this for example sawpping it for it's unicode character but we're gona be lazy and just reject any values that ask for it. Our username and password already do this as they do not allow characters outside of Alpha Numerical. We could apply this current validation to the first and second name but for the hell of it we'll create a new method. Add the following to our *"user.validation.js"* file:

```js
isSafe: function (input)
    {
        var regex = /([$])/;
        return !regex.test(input);
    }
```

As mentioned above all this does is return false if the input contains `$` and true otherwise. So now we just need to add our path methods as part of our user model:

Now inside our User model module we can import this module and using Mongoose's Schema we can attach validations method using `path()`. These are methods that are called before adding to the Database, if they fail then they are not persisted to the Database. So lets incorperate the above methods into our Schema validation:

```js
var validate = require('./user.validation');
...
/* include after Schema is created */
User.path('username').validate(function (input){
    return validate.isAlphaNumericOnly(input) && validate.isLongEnough(input);
});
User.path('password').validate(function (input){
    return validate.isGoodPassword(input) && validate.isLongEnough(input);
});
```


And boom! Now we can feel a little more confident in our stored data. One Final change you can make is to return a more meaningfull error. This is as 
simple as including an extra string variable in the Validate Method:

```js
User.path('firstname').validate(function (input){
    return validate.isSafe(input);
},"You Cannot use the '$' Character");
User.path('surname').validate(function (input){
    return validate.isSafe(input);
},"You Cannot use the '$' Character");
```

And just to makes sure we are returning the correct error message change the `POST` method's else in our routes to send the error message

```js
 response.send({ error: error+"" });
```

Whats happening here is mongoose has overridden the Prototype toString to return just the validation error message.

Here's what is should look like in Postman

![POSTing a new User](https://raw.githubusercontent.com/jonniedarko/Node-Resful-Api/master/screenshots/Postman%20screen%20shot%203.jpg)

[Code to this point](https://github.com/jonniedarko/Node-Resful-Api/commit/171b160cd785de1327749f247c9c0f9d5d5de8af)

Not so difficult eh? In the Next post we will deal with Encryption....Too many Companies are in hot water these days because they are storing sensitive information in plane text. I will show you how to apply basic encrytion to fields in out Database.
