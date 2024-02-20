<h1 align="center">Basic NodeJS Interview Question</h1>

### What is a Nodejs Library?
- Express
- Socket.io
- Async.js
- Request
- Ethers.js
- Mongoose
- UglifyJS2
- Jest
- Cors
- Passport
- Multer
- Gulp
- Browserify
- Parser
- Nodemailer
- Sequelize
- JSHint
- Axios
- Faker

### File structure of node js

nodejs-project/
│
├── node_modules/         (Automatically generated directory containing installed packages)
│
├── src/                   (Source code directory)
│   ├── index.js           (Entry point of the application)
│   ├── routes/            (Directory for route handlers)
│   │   ├── userRoutes.js  (Route handlers for user-related routes)
│   │   └── ...
│   ├── controllers/       (Directory for controller logic)
│   │   ├── userController.js  (Logic for handling user-related actions)
│   │   └── ...
│   ├── models/            (Directory for data models)
│   │   ├── userModel.js   (Model representing user data)
│   │   └── ...
│   ├── services/          (Directory for services)
│   │   ├── userService.js (Service for user-related business logic)
│   │   └── ...
│   └── ...
│
├── public/                (Directory for static assets, e.g., HTML, CSS, client-side JavaScript)
│   ├── index.html
│   ├── styles.css
│   └── ...
│
├── config/                (Directory for configuration files)
│   ├── db.js              (Database configuration)
│   ├── server.js          (Server configuration)
│   └── ...
│
├── tests/                 (Directory for tests)
│   ├── unit/              (Directory for unit tests)
│   ├── integration/       (Directory for integration tests)
│   └── ...
│
├── package.json           (File containing project metadata and dependencies)
├── package-lock.json      (File containing exact versions of dependencies)
├── .gitignore             (File specifying files and directories to ignore in Git)
└── README.md              (File containing project documentation)


### What is an event-loop in Node JS?

In Node.js, asynchronous operations are managed by the event loop using a queue and listener mechanism. When an asynchronous function needs to be executed, such as I/O operations, the main thread delegates it to a different thread, allowing V8 to continue executing the main code.

The event loop in Node.js involves different phases with specific tasks, including timers, pending callbacks, idle or prepare, poll, check, and close callbacks, each with its own First-In-First-Out (FIFO) queues. Between iterations, the event loop checks for asynchronous I/O or timers and shuts down cleanly if there aren't any pending tasks.

Understanding how the event loop manages asynchronous operations is crucial for writing efficient and non-blocking code in Node.js.

### Differentiate between process.nextTick() and setImmediate()?

Both can be used to switch to an asynchronous mode of operation by listener functions. 

process.nextTick() sets the callback to execute but setImmediate pushes the callback in the queue to be executed. So the event loop runs in the following manner

```javascript
// Timers phase
setTimeout(() => {
  console.log('Timer callback executed');
}, 1000);

// Pending callbacks phase
process.nextTick(() => {
  console.log('Pending callback executed');
});

// Connections phase (e.g., HTTP server)
const http = require('http');
const server = http.createServer((req, res) => {
  res.end('Hello World!');
});
server.listen(3000, () => {
  console.log('Server listening on port 3000');
});

// Check phase
setImmediate(() => {
  console.log('Immediate callback executed');
});

// Close callbacks phase
server.on('close', () => {
  console.log('Server closed');
});
```
In this process.nextTick() method adds the callback function to the start of the next event queue and setImmediate() method to place the function in the check phase of the next event queue.

### How can we use async await in node.js?

```javascript
// Retry with exponential backoff
function wait(timeout) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve();
    }, timeout);
  });
}

async function requestWithRetry(url) {
  const MAX_RETRIES = 10;
  for (let i = 0; i <= MAX_RETRIES; i++) {
    try {
      return await request(url);
    } catch (err) {
      const timeout = Math.pow(2, i);
      console.log('Waiting', timeout, 'ms');
      await wait(timeout);
      console.log('Retrying', err.message, i);
    }
  }
}
```
### What is an Event Emitter in Node.js?

EventEmitter is a Node.js class that includes all the objects that are basically capable of emitting events. 

This can be done by attaching named events that are emitted by the object using an eventEmitter.on() function. Thus whenever this object throws an even the attached functions are invoked synchronously.

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

myEmitter.on('event', () => {
  console.log('An event occurred!');
});

myEmitter.emit('event');
```

### What is middleware?

Middleware is the function that works between the request and the response cycle. Middleware gets executed after the server receives the request and before the controller sends the response.

### How does Node.js handle concurrency even after being single-threaded?

Node.js internally uses libuv library for handling all async call. This library creates multiple thread pools to handle async operations.

### Explain callback in Node.js.

A callback function is called after a given task. It allows other code to be run in the meantime and prevents any blocking.  Being an asynchronous platform, Node.js heavily relies on callback. All APIs of Node are written to support callbacks.

In Simple Terms :

In programming, a callback function is like a promise you make to do something later. Imagine you're cooking dinner, and you ask your friend to call you when they arrive. That call is like a callback - it happens later, when your friend is there.

Now, in Node.js (which is like a special kitchen for programming), everything works together without waiting around. When you ask Node.js to do something, like read a file or make a network request, it doesn't stop everything else. It just promises to let you know when it's done. That promise is the callback.

So, Node.js uses these callbacks everywhere. When you ask it to do something that might take time, you give it a callback function to call when it's finished. Meanwhile, Node.js keeps doing other stuff, like handling more requests or processing data. This way, nothing gets blocked, and everything keeps running smoothly.


### What is npm and its advantages?

NPM stands for Node Package Manager. It is an online repository for Node.js packages. We can install these packages in our projects/applications using the command line.

### What is callback hell in Node.js ?

Callback hell, also known as "pyramid of doom," is a situation in Node.js programming where you end up with deeply nested callback functions. This occurs when you have multiple asynchronous operations that depend on each other, and you handle them with nested callback functions. 

Imagine you have Task A, Task B, and Task C. Task B depends on the result of Task A, and Task C depends on the result of Task B. In traditional sequential programming

Example:

- Task A: Imagine you have to bake a cake. This is Task A.
- Task B: Task B could be decorating the cake with icing after it's baked. But you can only start decorating after the cake is baked (Task A).
- Task B: Task B could be decorating the cake with icing after it's baked. But you can only start decorating after the cake is baked (Task A).

```javascript
TaskA((resultA) => {
    TaskB(resultA, (resultB) => {
        TaskC(resultB, (resultC) => {
            // Do something with resultC
        });
    });
});
```
callback hell, you can use techniques like:

- Modularization: Break down your code into smaller, reusable functions to reduce nesting.

- Promises: Use Promises to handle asynchronous operations in a more sequential and readable manner.

- Async/Await: With async/await, you can write asynchronous code that looks synchronous, making it easier to understand and maintain.

### What are the clauses used in promise object in Node.js?

- Pending: The initial state of the promise before it is resolved or rejected.

- Fulfilled: The state of a promise representing a successful operation. This is also sometimes called "resolved."

- Rejected: The state of a promise representing a failed operation. To create a Promise object, you must pass a function (often called an executor function) to the Promise constructor.
This function takes two arguments: resolve and reject. These are functions that you call to either fulfill or reject the promise.


