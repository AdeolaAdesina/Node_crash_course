# Node_crash_course

https://www.youtube.com/watch?v=2LUdnb-mls0&t=293s



To check the version of node:

```
node --version
```

## Basic Hello World Example

Server.js:

```
const http = require("http");

const HOSTNAME = process.env.HOSTNAME || "localhost";
const PORT = process.env.PORT || 3000;

//then we create the server
const server = http.createServer((request, response) => {
    response.statusCode = 200;
    response.setHeader("Content-Type", "text/plain");
    response.end("Hello World");
})

//then we start the server
server.listen(PORT, HOSTNAME, () => (
    console.log(`Server is running at http://${HOSTNAME}:${PORT}/`)
));
```

http is one of the available modules in node.js.
Process is a global object, and within it we have our environmental variable.

Then we create the http server.

To run the code, we open up our terminal and run:

```
node server
```

![Screenshot_77](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/7e8fdfbf-0370-473c-bc19-7db5f7492636)


![Screenshot_78](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/993a0b21-b311-453c-bfe5-50e8e9658118)


![Screenshot_79](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/4bb84348-ce59-488d-bcf6-a05ba37c8539)


You just created your first webserver.

Stop the process in the terminal with Control + C.


## Global Properties 

We have access to other things other than process in the global object.

We also have:

global.__filename
global.__dirname


![Screenshot_80](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/11f4fbe9-c591-497c-9f92-7685a215a64e)

This will help when we need to read and write files on the server.


## File System Module

### Reading files

Create an hi.txt file and put:

```
Hello World
```

Now we're going to read it.

```
const { log } = require('console');
const fs = require('fs');

fs.readFile("hi.txt", (err, data) => {
    if (err) {
        console.error(err)
        return
    }
    console.log(data);
});
```


![Screenshot_81](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/9f12820f-d497-4a60-83dc-267fdda28789)



Notice that we're getting a data buffer not the actual text.


To get the text, we can add data.tostring().


![Screenshot_82](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/ac4a3f6e-6713-4d54-9279-1b6ff493af83)


Or we specify the encoding type.


![Screenshot_83](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/ae70cf8d-9ff1-44b4-868e-a38a905553ee)


Now let's add a console log outside our function.


![Screenshot_84](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/3a8def7c-8ff6-49d4-8b30-4927bf4d1642)


Do you notice that we got "Log from outside" before hello world?

This demonstrates the asynchronous nature of node. It takes node a split second to read the file, so it just process the second log first.

Each node module also has a synchronous version.


![Screenshot_85](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/102f1217-c173-4763-9b6a-f42692e6d0a6)


Instead of importing the entire fs module, we can also do destructuring.


![Screenshot_86](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/170ac47f-5389-4711-be13-7492d19450a3)



### Writing to Files


```
const { log } = require('console');
const { writeFile, writeFileSync } = require('fs');

const newContent = 'This is some new text'

writeFile('hi.txt', newContent, (err) => {
    if (err) {
        console.error(err);
        return
    }
    console.log('Content Written');
});
```


![Screenshot_87](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/91c463f6-3c09-4a79-957f-84b711e4eb87)



![Screenshot_88](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/9c35aab6-0bec-4686-835f-57063973bf17)


Notice that the content was entirely replaced.

That's the default behaviour of writeFile.

We can change this behaviour by adding a flag.


![Screenshot_89](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/124cead1-ea9d-4a1c-a4bc-d92f36566f2d)



![Screenshot_90](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/21ae41a6-4622-408c-bb4c-98c3fedec17e)


We can also use the synchronous version.

![Screenshot_91](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/34d60ccb-60c4-4de1-8678-07b30fa35b82)


![Screenshot_92](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/c844995c-9dc1-4d48-9864-719b334f020c)

Instead of using flag to append new text, we can use the appendFile option.


![Screenshot_93](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/f10791cd-af64-4611-8925-e58897c1b1de)

![Screenshot_94](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/b0e30990-ad0e-44bf-a99e-6a42fc18f239)



### Renaming and Deleting Files


```
const { log } = require('console');
const { rename } = require('fs');



rename('hi.txt', 'hello.txt', (err) => {
    if (err) {
        console.error(err);
        return
    }
    console.log('File Renamed');
})
```


![Screenshot_95](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/aaa52d9b-66d0-4886-85f5-18739f833a81)

![Screenshot_96](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/049c504f-0774-47f5-a45a-b6a6deaac325)




Deleting

```
const { log } = require('console');
const { unlink } = require('fs');



unlink('hello.txt', (err) => {
    if (err) {
        console.error(err);
        return
    }
    console.log('File Deleted');
})
```

![Screenshot_97](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/91ec167d-07cf-421f-82ee-3964fb6202e4)



## Importing and exporting modules.

server.js:

```
const addNums = require("./addNums");

const sum = addNums(2, 2)
console.log(sum);
```


addNums.js:

```
function addNums(a, b) {
    return a + b;
}

module.exports = addNums;
```


When you run it:

![Screenshot_98](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/63ca18eb-30d6-408c-82a4-c8fb8f814f31)


We can also use ES6 syntax.


![Screenshot_99](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/44032b32-1290-41aa-bccf-59f0d018b9a2)

![Screenshot_100](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/52adb35d-6853-40aa-a245-8870f64b133a)


Go get our package json:

```
npm init -y
```

This gave us a package.json then we added a new line: type: module

![Screenshot_101](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/1c0f7774-c727-40b0-8408-5ad79adc97b8)



## Basic HTML Content 

Now let's use the http module to create an actual webserver.

```
const http = require('http');

http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain')
    res.write('Hello World');
    res.end();
}).listen(3000, () => console.log('Server running'));
```

![Screenshot_102](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/e86a6a1c-5ea5-4408-a7eb-12419a285300)


![Screenshot_103](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/f683c4cb-8b56-4ced-9863-512e313a4a57)


![Screenshot_104](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/fa367ded-f8bb-47bc-9bc2-2796b20e6e3b)


We can refactor the code:

```
const http = require('http');

const PORT = process.env.PORT || 3000

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain')
    res.write('Hello World');
    res.write('Hello World again');
    res.end("The end");
})

server.listen(PORT, () => console.log(`Server is listening on port ${PORT}`));

```


![Screenshot_105](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/52a1eb75-731e-4afb-9144-2f713ec40dda)

![Screenshot_106](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/52a46943-1ba9-4043-b2c8-69d8dd162598)



This doesn't look great.


So let's look at sending some HTML.


We just change our content type to text/html, then get rid of res.write.

Then we have to restart our server.

```
const http = require('http');

const PORT = process.env.PORT || 3000

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/html')
    res.end("<h1>Hello World</h1>");
})

server.listen(PORT, () => console.log(`Server is listening on port ${PORT}`));
```


![Screenshot_107](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/5f5ddc04-959c-4f55-94ca-b5f3fc9dad52)

![Screenshot_108](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/5f2212ea-afef-460e-926f-c0fcf9669cd8)



## Webserver HTML Files

Now we want to send an HTML file and access that file.

So we use fs to access it.

```

```


index.html:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      body {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
        font-size: 5rem;
        background-color: darkslateblue;
        color: white;
      }
    </style>
    <title>Node.js Crash Course</title>
  </head>
  <body>
    <h1>Home</h1>
  </body>
</html>
```

![Screenshot_109](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/2c53d381-3eef-4bd3-8678-2ea5857f7f57)

![Screenshot_110](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/b72b0999-b850-46d5-80c8-4f6b74209785)



## Web Server Basic Routing

If we do http://localhost:3000/about 

We'll still get the same file.

Let's build a route to certain URL.

Now if we add this code to our server and refresh the browser, you will get:

![Screenshot_111](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/40607bdc-1fa8-4650-a59b-505607a47405)

![Screenshot_112](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/cf13edfe-a287-428e-a5aa-79d482194a03)

![Screenshot_114](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/2a6ffba1-b4d2-48b0-a0e1-7c79a3618a90)

![Screenshot_113](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/9de063e7-4282-4eac-a471-b40bf2c509c5)


We can now add this to the server code:

```
let path = './';
    switch(req.url) {
        case '/':
            path += 'index.html';
            break;
        case '/about':
            path += 'about.html';
            break;
        default:
            path += '404.html';
            break; 
    }
```


server.js:

```
const http = require('http');
const fs = require('fs');

const PORT = process.env.PORT || 3000

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/html');

    let path = './';
    switch(req.url) {
        case '/':
            path += 'index.html';
            break;
        case '/about':
            path += 'about.html';
            break;
        default:
            path += '404.html';
            break; 
    }

    fs.readFile(path, (err, data) => {
        if (err) {
            console.error(err);
            res.end();
        } else {
            res.write(data); // or res.end(data);
            res.end();
        }
    });
})

server.listen(PORT, () => console.log(`Server is listening on port ${PORT}`));
```


about.html:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      body {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
        font-size: 5rem;
        background-color: darkslateblue;
        color: white;
      }
    </style>
    <title>Node.js Crash Course</title>
  </head>
  <body>
    <h1>About</h1>
  </body>
</html>
```

404.html:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      body {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
        font-size: 5rem;
        background-color: darkslateblue;
        color: white;
      }
    </style>
    <title>Node.js Crash Course</title>
  </head>
  <body>
    <h1>404</h1>
  </body>
</html>
```


Now run the server with ```node server```



Then add the status codes:

```
const http = require('http');
const fs = require('fs');

const PORT = process.env.PORT || 3000

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/html');

    let path = './';
    switch(req.url) {
        case '/':
            path += 'index.html';
            res.statusCode = 200;
            break;
        case '/about':
            path += 'about.html';
            res.statusCode = 200;
            break;
        default:
            path += '404.html';
            res.statusCode = 404;
            break; 
    }

    fs.readFile(path, (err, data) => {
        if (err) {
            console.error(err);
            res.end();
        } else {
            res.write(data); // or res.end(data);
            res.end();
        }
    });
})

server.listen(PORT, () => console.log(`Server is listening on port ${PORT}`));
```



## Web Server Redirects

We can redirect them back to homepage instead of 404.

So the default will be:

```
default:
            res.setHeader('Location', '/')
            res.statusCode = 301;
            break;
```

Now when you run the server, any random URl will take you back to the home page.



## NPM - Node Package manager

```
npm init
```

This creates a package.json


## Nodemon

with Nodemon, we don't need to restart our server every time we make changes.

```
npm i -g nodemon
```

This will install nodemon globally on our computer.

```
npm i -D nodemon
```

This will install nodemon as a dev dependency on our project.

Now look in your package.json.


You will see your node_modules folder now.

Now create a gitignore file to hide your node_modules file

![Screenshot_117](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/1295f33c-03b5-4b74-9326-03f3abefefae)

If we were to download this from github, we will get a node modules folder.

So i can reinstall the dependencies with

```
npm i
```

Now we can edit the scripts in the package.json:

```
"dev": "nodemon server"
"start": "node server"
```

Now we can start using nodemon with:

```
npm run dev
```

![Screenshot_118](https://github.com/AdeolaAdesina/Node_crash_course/assets/29931071/3c928e58-dc65-4d16-a8c1-317b7dc8713a)


Now let's change the default back to 404.

```
default:
            path += '404.html';
            res.statusCode = 404;
            break;
```

Save and see that nodemon works.

