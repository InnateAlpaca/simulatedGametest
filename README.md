# simulatedGametest
Test bedrock scripting API classes and functions on nodejs before using them on your addon project. Get the hang of it and easily find bugs.

## What is it?
simulatedGametest is a collection of classes, functions and constants that have the same behaviour and signature as modules form official [bedrock scripting API](https://learn.microsoft.com/en-us/minecraft/creator/scriptapi/). These classes can be imported, and run with [NodeJS](https://nodejs.org/en/), making it very easy to test and debug parts of your code directly from node. This is thought especially for those who are learning about server-net and server-admin modules and for people who, in general, don't know much about bedrock-scripting for servers.

Having the same signature as official scripting, once you have fixed your server dynamics (connection between your local server and mc-server) you can easily copy and paste your code to your addon project (changing the `import` statement ofcourse), and it will work without any change.
Furthermore, if you learned http request handling with bedrock-scripting, and you want to code more outsite bedrock (some node app maybe) without learning form scratch how to use a new http-module, you can still use it!

Finally this library doesn't use any external class or module. You don't need to install anything more than just this very code.

## Examples
So, you are starting to code a bedrock->server service, and before adding all other stuff related to game, you want to make sure that both your scripting code and server-code are compatibles. Or you may even have just written it, but something is wrong with server connection... (example taken from microsoft [docs](https://learn.microsoft.com/en-us/minecraft/creator/scriptapi/minecraft/server-net/httprequest))

Here's the code for the server, which we run by `node server.js` (supposing it is saved in a file called `server.js`).
```
const {createServer} = require('http')

let score = 0;
const node_server = createServer((req, res)=>{
    if (req.url=='/updateScore'){
        req.on('data', data=>{
            score = JSON.parse(data).score;
            res.writeHead(200, { 'content-type': 'text/plain' });
        })
    }
})
node_server.listen(3000);
```
And here the code for the client, which we run by `node client.js` (assuming that it's saved on a file called  `client.js`)
```
// simulated client code
const {http, HttpRequest, HttpHeader, HttpRequestMethod} = require('./simulatedGametest');

const req = new HttpRequest("http://localhost:3000/updateScore");
req.body = JSON.stringify({
    score: 22,
});
req.method = HttpRequestMethod.POST;
req.headers = [
    new HttpHeader("Content-Type", "application/json"),
    new HttpHeader("auth", "my-auth-token"),
];

const response = await http.request(req);
```
