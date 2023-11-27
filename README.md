# Node_crash_course

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






