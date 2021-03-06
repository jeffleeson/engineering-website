{{{
  "title" : "Organizing client-side JavaScript with Browserify",
  "authorName": "Oren",
  "authorLink": "https://github.com/oren",
  "authorImage": "https://secure.gravatar.com/avatar/ea28a1533185f15e9364a8db6f9c0bae?s=140&d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png",
  "tags" : [ "tech" ],
  "date" : "11-5-2012"
}}}

![browserify](http://substack.net/doc/hujs/07_browserify.png)

How do you organize your client side js code?  
The node.js guys adopted the CommonJS approach - each function in it's own file  
and you simply require the file you want to use. here is a simple example:

saveUser.js 

    # this file contain one function that saves user info in our Database

    module.exports = function(userId) {
      // save user in DB
      console.log('user' + userId + ' saved in DB');
    };
   
app.js

    # require is the way to use a module in node.js. 
    # a module can be a bulit-in one or our own, similar to what we do here

    var foo = require('./saveUser.js');  # foo contains a function that can save our user
    foo(1);                              # calling our function with a user id

great for code reuse and easy to test, right? true, but how is that relevant to client-side js?  
I am glad you asked. I use a tool called [browserify](https://github.com/substack/node-browserify) that let me use CommonJS in the browser!  
let's jump right in and show you how to use it.
go ahead and create app.js and saveUser.js from the code samples above.  

now add a simple index.html that uses app.js

    <!DOCTYPE html>
    <html lang="en">
      <head>
      </head>

      <body>
        <p>CommonJS in the browser!</p>
       
         <script src="app.js"></script>
      </body>
    </html>

if you look at the browser's console you will notice an error: `Uncaught ReferenceError: require is not defined`   
that makes sense, since require is not available for js client side. let's fix that with browserify.

browserify is a node.js package so just like any other node package, you got to have [node.js](http://nodejs.org) on your machine and you should use npm to install it:

    npm install browserify -g

adding -g tells node to install this package globaly - it will available anywhere and not only in the current directory.

lets see what happend when we give it one argument, our js file:

    browserify app.js

we see a long javascript code printed in the console. browserify did it's magic and wrap our file so it will be able to use our require function.  
that's nice but we need to save the output into a file, right? lets do that with -o

    browserify app.js -o bundle.js

now we can use bundle.js insted of app.js in our html file:

    <!DOCTYPE html>
    <html lang="en">
      <head>
      </head>

      <body>
        <p>CommonJS in the browser!</p>

        <script src="bundle.js"></script>
      </body>
    </html>

if you see "user saved in DB" in the browser's console, everything is fine.

that's cool but I'm not going do this browserify dance every time I make a change to my js.  
right, that's why you can use -w to watch for changes and generate the bundle file:

    browserify app.js -o bundle.js -w

that's it, you can leave the spaghetti for the kitchen and enjoy mess-free js code.  

(All code samples for are available [here](https://github.com/oren/oren.github.com/tree/master/posts/browserify))
