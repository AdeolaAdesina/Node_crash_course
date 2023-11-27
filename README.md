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



