# Node.js & Express.js — 205 Frequently Asked Questions

> Comprehensive FAQ covering basics to advanced topics for Node.js and Express.js

---

## Table of Contents

1. [Node.js Basics](#nodejs-basics)
2. [Node.js Modules](#nodejs-modules)
3. [Streams & Buffers](#streams--buffers)
4. [HTTP & Networking](#http--networking)
5. [Error Handling](#error-handling)
6. [Child Processes](#child-processes)
7. [Performance](#performance)
8. [Security](#security)
9. [Express.js Basics](#expressjs-basics)
10. [Express.js Routing](#expressjs-routing)
11. [Template Engines](#template-engines)
12. [Authentication](#authentication)
13. [Database](#database)
14. [REST API](#rest-api)
15. [Testing](#testing)
16. [Deployment](#deployment)
17. [Express.js Advanced](#expressjs-advanced)
18. [Node.js Advanced](#nodejs-advanced)
19. [Microservices & Architecture](#microservices--architecture)
20. [Logging & Monitoring](#logging--monitoring)
21. [Miscellaneous](#miscellaneous)

---

## Node.js Basics

**Q1. What is Node.js?**
> Node.js is an open-source, cross-platform JavaScript runtime environment that executes JavaScript code outside the browser. It is built on Chrome's V8 JavaScript engine and uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

---

**Q2. Who created Node.js and when?**
> Node.js was created by Ryan Dahl in 2009. He introduced it to address the limitations of the Apache HTTP server in handling large numbers of concurrent connections.

---

**Q3. What is the V8 engine?**
> V8 is Google's open-source high-performance JavaScript and WebAssembly engine, written in C++. It is used in Google Chrome and also in Node.js to execute JavaScript code outside of a browser environment.

---

**Q4. What is the difference between Node.js and JavaScript?**
> JavaScript is a programming language, while Node.js is a runtime environment that allows JavaScript to run on the server side. Node.js provides additional APIs for file system access, networking, and other system-level operations not available in browser-based JavaScript.

---

**Q5. What is npm?**
> npm (Node Package Manager) is the default package manager for Node.js. It is used to install, share, and manage dependencies (packages/modules) in Node.js projects. It also provides a registry hosting thousands of open-source packages.

---

**Q6. What is package.json?**
> package.json is a metadata file in a Node.js project that contains project details like name, version, description, scripts, and dependencies. It is used by npm to manage the project's packages and scripts.

---

**Q7. What is the difference between dependencies and devDependencies?**
> `dependencies` are packages required for the application to run in production, while `devDependencies` are packages needed only during development (e.g., testing frameworks, linters, build tools). devDependencies are not installed when `NODE_ENV=production`.

---

**Q8. What is the event loop in Node.js?**
> The event loop is the core mechanism that allows Node.js to perform non-blocking I/O operations. It continuously checks the call stack and the callback queue, executing callbacks when the call stack is empty. It enables Node.js to handle many concurrent operations with a single thread.

---

**Q9. Is Node.js single-threaded?**
> Yes, Node.js runs JavaScript code in a single thread. However, it uses worker threads for I/O operations internally through libuv, allowing it to handle many concurrent connections without blocking. The Worker Threads module also allows running JavaScript in parallel.

---

**Q10. What is libuv?**
> libuv is a multi-platform C library that provides asynchronous I/O support, thread pool, event loop, and other utilities. Node.js uses libuv to handle asynchronous operations across different operating systems.

---

**Q11. What are the phases of the Node.js event loop?**
> The event loop phases are: **timers** (setTimeout/setInterval), **pending callbacks**, **idle/prepare** (internal), **poll** (I/O events), **check** (setImmediate), and **close callbacks**. Each phase processes callbacks in its own queue before moving to the next.

---

**Q12. What is the difference between process.nextTick() and setImmediate()?**
> `process.nextTick()` fires before any I/O events and before `setImmediate()`. It adds the callback to the "next tick queue" which is processed at the end of the current operation before the event loop continues. `setImmediate()` fires in the check phase of the event loop, after I/O events.

---

**Q13. What is a callback function in Node.js?**
> A callback is a function passed as an argument to another function, to be executed after an asynchronous operation completes. Node.js uses the error-first callback pattern where the first parameter is an error object (null if no error) followed by result parameters.

---

**Q14. What is callback hell?**
> Callback hell (also called pyramid of doom) refers to heavily nested callbacks that make code hard to read and maintain. It occurs when multiple asynchronous operations depend on each other. It can be mitigated using Promises, async/await, or modularizing code.

---

**Q15. What are Promises in Node.js?**
> A Promise is an object representing the eventual completion or failure of an asynchronous operation. It has three states: pending, fulfilled (resolved), or rejected. Promises provide `.then()` and `.catch()` methods to handle results and errors, avoiding callback hell.

---

**Q16. What is async/await in Node.js?**
> async/await is syntactic sugar built on top of Promises that allows writing asynchronous code in a synchronous style. An `async` function always returns a Promise, and `await` pauses execution inside an async function until the Promise resolves or rejects.

---

**Q17. What is the global object in Node.js?**
> In Node.js, the global object is `global`, which provides globally available variables and functions such as `setTimeout`, `setInterval`, `Buffer`, `process`, and `console`. Unlike browser JavaScript, variables declared without var/let/const are NOT automatically added to global.

---

**Q18. What is the process object in Node.js?**
> The `process` object is a global object that provides information about, and control over, the current Node.js process. It exposes properties like `process.env` (environment variables), `process.argv` (command-line arguments), `process.pid`, and methods like `process.exit()`.

---

**Q19. How do you read environment variables in Node.js?**
> Environment variables are accessed via `process.env`. For example, `process.env.NODE_ENV` returns the value of the NODE_ENV variable. The `dotenv` package is commonly used to load variables from a `.env` file into `process.env` during development.

---

**Q20. What is the __dirname and __filename variable?**
> `__dirname` is a string containing the absolute path of the directory containing the currently executing file. `__filename` contains the absolute path of the currently executing file. Note: these are not available in ES modules; use `import.meta.url` instead.

---

## Node.js Modules

**Q21. What is the CommonJS module system?**
> CommonJS is the module system used by Node.js by default. It uses `require()` to import modules and `module.exports` or `exports` to export them. Each file is treated as a separate module with its own scope, preventing global namespace pollution.

---

**Q22. What is the difference between require() and import?**
> `require()` is the CommonJS way to import modules (synchronous, loads at runtime). `import` is the ES Module (ESM) syntax (can be asynchronous, parsed at load time). Node.js supports both, but they cannot be freely mixed. ESM requires `.mjs` extension or `"type":"module"` in package.json.

---

**Q23. What is module.exports vs exports?**
> `module.exports` is the actual object returned when `require()` is called. `exports` is a reference to `module.exports` initially. You can add properties to `exports`, but if you reassign `exports` entirely, it breaks the reference to `module.exports`. Always use `module.exports` for reassigning the entire export.

---

**Q24. What is the require cache?**
> Node.js caches required modules after the first load, storing them in `require.cache`. Subsequent `require()` calls for the same module return the cached version without re-executing the module file. You can delete entries from `require.cache` to force re-loading.

---

**Q25. What are built-in/core modules in Node.js?**
> Core modules are built into Node.js and do not need to be installed via npm. Examples include `fs` (file system), `http`, `https`, `path`, `os`, `events`, `stream`, `crypto`, `url`, `querystring`, and `util`. They are loaded by passing just the module name to `require()`.

---

**Q26. What is the path module?**
> The `path` module provides utilities for working with file and directory paths in a cross-platform way. Common methods include `path.join()`, `path.resolve()`, `path.dirname()`, `path.basename()`, `path.extname()`, and `path.parse()`.

---

**Q27. What is the fs module?**
> The `fs` (file system) module provides APIs to interact with the file system. It supports reading (`fs.readFile`), writing (`fs.writeFile`), appending, deleting, and watching files and directories. Both synchronous and asynchronous versions are available.

---

**Q28. What is the os module?**
> The `os` module provides operating system-related utility methods and properties. It includes `os.platform()`, `os.arch()`, `os.cpus()`, `os.totalmem()`, `os.freemem()`, `os.homedir()`, `os.tmpdir()`, and `os.hostname()`.

---

**Q29. What is the events module?**
> The `events` module provides the `EventEmitter` class, which is the foundation of Node.js's event-driven architecture. Objects that emit events are instances of EventEmitter. You can use `.on()` to listen for events and `.emit()` to trigger them.

---

**Q30. What is the crypto module?**
> The `crypto` module provides cryptographic functionality including hash generation, HMAC, cipher/decipher, sign/verify operations, and secure random bytes. Common uses include hashing passwords and generating tokens using algorithms like SHA256, MD5, and AES.

---

## Streams & Buffers

**Q31. What are streams in Node.js?**
> Streams are objects that allow reading or writing data piece by piece (chunk by chunk) rather than loading the entire data into memory. There are four types: **Readable**, **Writable**, **Duplex** (both), and **Transform** (duplex with data transformation).

---

**Q32. What is piping in Node.js streams?**
> Piping is a mechanism to connect the output of a Readable stream directly to the input of a Writable stream. The `pipe()` method handles data flow automatically. Example: `readableStream.pipe(writableStream)`. It is useful for efficiently processing large files.

---

**Q33. What is the Buffer class in Node.js?**
> `Buffer` is a built-in class used to handle binary data directly in Node.js. Buffers are fixed-size chunks of memory allocated outside the V8 heap. They are used when working with TCP streams, file system operations, or any scenario involving raw binary data.

---

**Q34. How do you convert a Buffer to a string?**
> Use `buffer.toString(encoding)` where encoding can be `'utf8'`, `'ascii'`, `'base64'`, `'hex'`, etc. The default encoding is `'utf8'`. Example: `Buffer.from('hello').toString('utf8')` returns `'hello'`.

---

**Q35. What is backpressure in streams?**
> Backpressure occurs when a Writable stream cannot consume data as fast as a Readable stream produces it. Node.js stream APIs signal backpressure via return values of `.write()` (returns `false` when buffer is full). The `'drain'` event indicates the writable stream is ready for more data.

---

## HTTP & Networking

**Q36. How do you create an HTTP server in Node.js?**
> Use the built-in `http` module:
> ```js
> const http = require('http');
> const server = http.createServer((req, res) => { res.end('Hello'); });
> server.listen(3000);
> ```
> The callback receives `IncomingMessage` (req) and `ServerResponse` (res) objects.

---

**Q37. What is the difference between http and https modules?**
> The `http` module creates plain HTTP servers and clients. The `https` module creates SSL/TLS-secured servers and clients. To use https, you need an SSL certificate and private key. Both share similar APIs but `https.createServer()` requires options with `key` and `cert`.

---

**Q38. How do you make HTTP requests in Node.js?**
> You can use the built-in `http`/`https` module's `request()` method, or popular packages like `axios`, `node-fetch`, or `got`. Example with axios: `const response = await axios.get('https://api.example.com/data');`

---

**Q39. What is the net module in Node.js?**
> The `net` module provides an asynchronous network API for creating TCP or IPC servers and clients. It is a lower-level module that the `http` module is built on top of. Use `net.createServer()` and `net.createConnection()` for raw TCP communication.

---

**Q40. What is WebSocket and how do you use it in Node.js?**
> WebSocket is a communication protocol providing full-duplex communication channels over a single TCP connection. In Node.js, the popular `ws` package or Socket.io library is used to implement WebSocket servers and clients for real-time applications.

---

## Error Handling

**Q41. How do you handle errors in Node.js?**
> Error handling methods include: `try/catch` for synchronous code, error-first callbacks for async operations, `.catch()` on Promises, `try/catch` inside async/await functions, and the `'error'` event on EventEmitters. For uncaught exceptions, use `process.on('uncaughtException', ...)` as a last resort.

---

**Q42. What is the difference between operational and programmer errors?**
> Operational errors are expected runtime errors like network failures, invalid user input, or file not found. Programmer errors are bugs in the code like undefined variables, type mismatches, or logic errors. Operational errors should be handled gracefully; programmer errors should be fixed.

---

**Q43. What is process.on('uncaughtException')?**
> It is a process event handler for uncaught exceptions that would otherwise crash the Node.js process. While it can log the error and perform cleanup, it is dangerous to continue running after an uncaught exception. It is best used for logging before gracefully shutting down.

---

**Q44. What is process.on('unhandledRejection')?**
> It handles Promise rejections that were not caught with `.catch()` or `try/catch`. In newer versions of Node.js, unhandled rejections terminate the process by default. Use it to log errors and exit gracefully:
> ```js
> process.on('unhandledRejection', (reason) => { console.error(reason); process.exit(1); });
> ```

---

**Q45. What is the Error class in Node.js?**
> The built-in `Error` class creates error objects with `message` and `stack` properties. You can extend it to create custom error classes. Common built-in error types include `TypeError`, `RangeError`, `SyntaxError`, `ReferenceError`, and `EvalError`.

---

## Child Processes

**Q46. What is the child_process module?**
> The `child_process` module allows Node.js to create and manage child processes. It provides `spawn()`, `exec()`, `execFile()`, and `fork()` methods to run external commands or other Node.js scripts in separate processes, enabling parallelism and access to system commands.

---

**Q47. What is the difference between spawn and exec?**
> `spawn()` starts a process and returns a stream for stdout/stderr — suitable for large outputs or long-running processes. `exec()` buffers the entire output and provides it in a callback — suitable for short commands with limited output. `spawn()` is more efficient for large data.

---

**Q48. What is the cluster module in Node.js?**
> The `cluster` module allows creating multiple worker processes that share the same server port. The master process forks worker processes, and incoming connections are distributed among them. This leverages multi-core systems to improve performance since Node.js is single-threaded.

---

**Q49. What are Worker Threads in Node.js?**
> Worker Threads (available via the `worker_threads` module) allow running JavaScript in parallel threads. Unlike child processes, workers share memory using `SharedArrayBuffer`. They are ideal for CPU-intensive tasks that would block the event loop, like heavy computation.

---

## Performance

**Q50. How do you improve Node.js performance?**
> Performance improvements include: using clustering or worker threads for CPU-intensive tasks, caching frequently accessed data, using streams instead of buffering large data, optimizing database queries, enabling compression (gzip), using a load balancer, and profiling with Node.js built-in tools.

---

**Q51. What is the Node.js profiler?**
> Node.js includes a built-in profiler based on V8's profiler. Run `node --prof app.js` to generate a tick file, then use `node --prof-process` to analyze it. Tools like clinic.js, 0x, and Chrome DevTools can also profile Node.js applications.

---

**Q52. What is memory leak in Node.js and how to detect it?**
> A memory leak occurs when the application retains references to objects that are no longer needed, preventing garbage collection. Detection tools include Node.js's built-in `--inspect` flag with Chrome DevTools, `heapdump`, `memwatch-next`, and `clinic.js`.

---

**Q53. What is the --inspect flag in Node.js?**
> The `--inspect` flag enables the V8 inspector protocol, allowing you to debug Node.js applications using Chrome DevTools or any compatible debugger. Run `node --inspect app.js` and open Chrome at `chrome://inspect` to attach the debugger.

---

## Security

**Q54. What are common security threats in Node.js applications?**
> Common threats include: injection attacks (SQL, NoSQL, command injection), Cross-Site Scripting (XSS), Cross-Site Request Forgery (CSRF), insecure deserialization, broken authentication, sensitive data exposure, and denial-of-service (DoS) attacks.

---

**Q55. How do you prevent SQL injection in Node.js?**
> Use parameterized queries or prepared statements instead of string concatenation. ORMs like Sequelize and Mongoose also help prevent injection. For raw queries, always use the database driver's built-in parameterization mechanisms.

---

**Q56. What is Helmet.js?**
> Helmet.js is a collection of middleware functions for Express.js that sets various HTTP headers to improve app security. It helps protect against well-known web vulnerabilities by setting headers like `Content-Security-Policy`, `X-Frame-Options`, and `X-Content-Type-Options`.

---

**Q57. How do you hash passwords in Node.js?**
> Use the `bcrypt` package (or `bcryptjs`). Never store plain-text passwords. Example: `const hash = await bcrypt.hash(password, 10);` to hash, and `bcrypt.compare(password, hash)` to verify. bcrypt automatically handles salting.

---

**Q58. What is rate limiting and how do you implement it?**
> Rate limiting restricts the number of requests a client can make in a time window, preventing brute-force and DoS attacks. Use the `express-rate-limit` middleware:
> ```js
> app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }));
> ```

---

## Express.js Basics

**Q59. What is Express.js?**
> Express.js is a minimal and flexible Node.js web application framework that provides a robust set of features for building web and mobile applications. It simplifies server creation, routing, middleware handling, and template rendering on top of Node.js's HTTP module.

---

**Q60. Who created Express.js?**
> Express.js was created by TJ Holowaychuk in 2010. It is now maintained by the Node.js Foundation and is one of the most popular Node.js frameworks.

---

**Q61. How do you install Express.js?**
> Install Express.js using npm: `npm install express`. Then import it with `const express = require('express');` and create an app with `const app = express();`

---

**Q62. How do you create a basic Express server?**
> ```js
> const express = require('express');
> const app = express();
> app.get('/', (req, res) => { res.send('Hello World!'); });
> app.listen(3000, () => console.log('Server running on port 3000'));
> ```

---

**Q63. What is middleware in Express.js?**
> Middleware are functions that have access to the request (`req`), response (`res`), and `next()` function in the request-response cycle. They can execute code, modify req/res objects, end the cycle, or call `next()` to pass control to the next middleware.

---

**Q64. What are the types of middleware in Express?**
> Express has five types: **Application-level** (`app.use()`), **Router-level** (`router.use()`), **Error-handling** (four parameters: err, req, res, next), **Built-in** (`express.json`, `express.static`), and **Third-party** (e.g., morgan, cors).

---

**Q65. What is the next() function in Express middleware?**
> `next()` passes control to the next middleware in the stack. If the current middleware does not end the request-response cycle, it must call `next()` to avoid hanging the request. Calling `next(err)` passes control to error-handling middleware.

---

**Q66. What is the difference between app.use() and app.get()?**
> `app.use()` mounts middleware for all HTTP methods and matches any path starting with the specified path. `app.get()` defines a route handler specifically for HTTP GET requests matching an exact path. `app.use()` is for middleware; `app.get/post/put/delete` are for route handling.

---

**Q67. How do you serve static files in Express?**
> Use the built-in `express.static()` middleware: `app.use(express.static('public'))`. This serves files from the `public` directory. You can optionally add a virtual path prefix: `app.use('/static', express.static('public'))`.

---

**Q68. What is express.json() middleware?**
> `express.json()` is built-in middleware that parses incoming requests with JSON payloads. It makes the parsed data available under `req.body`. It must be registered before any routes that need to access request body data.

---

**Q69. What is express.urlencoded() middleware?**
> `express.urlencoded()` parses incoming requests with URL-encoded payloads (like HTML form submissions). Use it with `{ extended: true }` to support rich objects, or `{ extended: false }` for simple key-value pairs using the querystring library.

---

**Q70. How do you handle 404 errors in Express?**
> Add a middleware at the end of all routes:
> ```js
> app.use((req, res, next) => { res.status(404).send('Not Found'); });
> ```
> Since no route matched before this point, it acts as a catch-all 404 handler.

---

**Q71. How do you handle errors in Express?**
> Define an error-handling middleware with four parameters:
> ```js
> app.use((err, req, res, next) => { res.status(500).json({ error: err.message }); });
> ```
> Express identifies it as an error handler by its four-parameter signature. Register it after all other middleware and routes.

---

**Q72. What is the Request object in Express?**
> The `req` object represents the HTTP request and contains properties like `req.params` (route parameters), `req.query` (query string), `req.body` (request body), `req.headers`, `req.method`, `req.url`, `req.cookies`, and `req.ip`.

---

**Q73. What is the Response object in Express?**
> The `res` object represents the HTTP response. It provides methods like `res.send()`, `res.json()`, `res.status()`, `res.redirect()`, `res.render()`, `res.download()`, `res.set()` (set headers), `res.cookie()`, and `res.sendFile()`.

---

**Q74. What is res.send() vs res.json()?**
> `res.send()` can send strings, Buffers, or objects (auto-converted to JSON). `res.json()` always sends a JSON response with `Content-Type: application/json`. `res.json()` also applies JSON-specific settings like JSON.stringify replacer/spaces.

---

**Q75. What is res.locals in Express?**
> `res.locals` is an object that contains response local variables scoped to a single request-response cycle. It is useful for passing data from middleware to route handlers or templates. Variables set on `res.locals` are available in views rendered during that request.

---

**Q76. What is app.locals in Express?**
> `app.locals` is an object whose properties persist throughout the life of the application. They are available in all views rendered by the application. Use them for application-wide settings or data like the app name or site configuration.

---

## Express.js Routing

**Q77. What is routing in Express.js?**
> Routing defines how an application responds to a client request at a particular endpoint (URL) and HTTP method. Express supports route methods like `app.get()`, `app.post()`, `app.put()`, `app.delete()`, and `app.all()` for all methods.

---

**Q78. What are route parameters?**
> Route parameters are named URL segments prefixed with a colon (`:`) that capture values from the URL. For example, in `app.get('/users/:id', ...)`, `req.params.id` will contain the value from the URL. Multiple parameters can be defined: `/users/:userId/books/:bookId`.

---

**Q79. What is query string in Express?**
> Query strings are key-value pairs appended to the URL after a `?` mark (e.g., `/search?name=john&age=25`). In Express, they are automatically parsed and available via `req.query` object (e.g., `req.query.name === 'john'`).

---

**Q80. What is Express Router?**
> `express.Router` is a mini application object capable of performing middleware and routing functions. It is used to modularize routes into separate files. Create with `const router = express.Router()`, define routes on it, then mount it with `app.use('/prefix', router)`.

---

**Q81. What is the difference between app.route() and app.get()/app.post()?**
> `app.route()` creates a chainable route handler for a single path, allowing you to define multiple HTTP method handlers in one place:
> ```js
> app.route('/user').get(...).post(...).put(...)
> ```
> This avoids repetition and makes the code more readable.

---

**Q82. What are route wildcards in Express?**
> Express supports string pattern matching in route paths. `*` matches any characters. For example, `'/ab*cd'` matches `'/abcd'`, `'/abXcd'`, `'/ab123cd'`. You can also use regular expressions as route paths.

---

**Q83. How do you implement RESTful routes in Express?**
> RESTful routes follow conventions: `GET /resources` (list), `POST /resources` (create), `GET /resources/:id` (get one), `PUT /resources/:id` (update), `DELETE /resources/:id` (delete). Organize these in a router file and mount with `app.use('/resources', resourceRouter)`.

---

**Q84. What is the order of route matching in Express?**
> Express matches routes in the order they are defined. The first matching route handler is executed. If that handler calls `next()`, Express continues to the next matching route. Order matters — more specific routes should be defined before more general ones.

---

**Q85. How do you redirect in Express?**
> Use `res.redirect([status,] url)`. Express defaults to HTTP 302 (Found). For permanent redirect: `res.redirect(301, '/new-url')`. You can also use relative paths, absolute paths, or external URLs.

---

**Q86. What is router.param() in Express?**
> `router.param()` adds callback triggers to route parameters. It is called when a parameter appears in a route. For example:
> ```js
> router.param('id', (req, res, next, id) => { /* fetch user by id */ next(); });
> ```
> This runs automatically when a route has an `:id` parameter.

---

## Template Engines

**Q87. What are template engines in Express?**
> Template engines enable dynamic HTML generation by combining template files with data. Express supports EJS, Pug (Jade), Handlebars, and Mustache. Set the engine with `app.set('view engine', 'ejs')` and render with `res.render('viewName', data)`.

---

**Q88. What is EJS?**
> EJS (Embedded JavaScript) is a templating engine that lets you embed JavaScript inside HTML files using `<% %>` (script), `<%= %>` (output), and `<%- %>` (unescaped output) tags. It is one of the most popular template engines for Express due to its simplicity.

---

**Q89. What is Pug (formerly Jade)?**
> Pug is a high-performance template engine that uses indentation-based syntax instead of HTML tags. It compiles to HTML and supports variables, conditionals, loops, and includes. It results in cleaner, less verbose templates but has a learning curve for those used to HTML.

---

**Q90. How do you set views directory in Express?**
> Use `app.set('views', path.join(__dirname, 'views'))` to specify the directory where template files are located. Express uses this path when `res.render()` is called with a view name.

---

## Authentication

**Q91. What is Passport.js?**
> Passport.js is authentication middleware for Node.js and Express. It supports over 500 authentication strategies including local (username/password), OAuth (Google, Facebook, Twitter), JWT, and more. It is modular — install only the strategies you need.

---

**Q92. What is JWT (JSON Web Token)?**
> JWT is an open standard (RFC 7519) for securely transmitting information as a JSON object. A JWT consists of three parts: **Header** (algorithm), **Payload** (claims/data), and **Signature**, separated by dots. It is widely used for stateless authentication in REST APIs.

---

**Q93. How do you implement JWT authentication in Express?**
> Use the `jsonwebtoken` package: generate a token with `jwt.sign(payload, secret, { expiresIn })` and verify with `jwt.verify(token, secret)`. Create middleware to extract the token from the `Authorization: Bearer <token>` header and verify it before protected routes.

---

**Q94. What is session-based authentication vs JWT?**
> Session-based auth stores session data on the server and sends a session ID cookie to the client. JWT auth stores all data in the token on the client — the server is stateless. Sessions are easier to invalidate; JWTs are more scalable for distributed systems.

---

**Q95. What is express-session?**
> `express-session` is middleware for managing sessions in Express. It stores session data on the server and sends a session ID cookie to the client. Configuration includes a secret for signing cookies, `resave` (save session on every request), and `saveUninitialized` options.

---

**Q96. What is bcrypt and why is it used?**
> bcrypt is a password-hashing function designed to be computationally expensive, making brute-force attacks difficult. The `bcrypt` npm package allows hashing passwords with a configurable cost factor (salt rounds). Always hash passwords before storing them.

---

**Q97. What is OAuth 2.0?**
> OAuth 2.0 is an authorization framework that enables applications to obtain limited access to user accounts on third-party services (like Google, GitHub) without exposing credentials. It uses access tokens. Passport.js strategies like `passport-google-oauth20` simplify implementation.

---

## Database

**Q98. What is Mongoose?**
> Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a schema-based solution to model data, built-in type casting, validation, query building, and business logic hooks. It sits on top of the official MongoDB Node.js driver.

---

**Q99. What is Sequelize?**
> Sequelize is a promise-based ORM for Node.js that supports PostgreSQL, MySQL, MariaDB, SQLite, and MSSQL. It provides model definitions, associations, migrations, and a query interface without writing raw SQL.

---

**Q100. What is connection pooling?**
> Connection pooling maintains a pool of reusable database connections to avoid the overhead of opening and closing a connection for each request. Most database clients (like `pg` for PostgreSQL) support pooling. Configure pool size based on your application's concurrency needs.

---

**Q101. What is Prisma?**
> Prisma is a next-generation ORM for Node.js and TypeScript. It consists of Prisma Client (auto-generated type-safe database client), Prisma Migrate (schema migrations), and Prisma Studio (visual database editor). It supports PostgreSQL, MySQL, SQLite, and MongoDB.

---

**Q102. What is Knex.js?**
> Knex.js is a SQL query builder for Node.js supporting PostgreSQL, MySQL, SQLite3, and MSSQL. Unlike full ORMs, Knex focuses on building and executing SQL queries with a fluent API while giving more control over raw SQL. It also supports migrations and seeding.

---

**Q103. How do you connect to MongoDB in Node.js?**
> Use Mongoose:
> ```js
> mongoose.connect('mongodb://localhost:27017/dbname', { useNewUrlParser: true, useUnifiedTopology: true });
> ```
> Or use the official `mongodb` driver: `const client = new MongoClient(uri); await client.connect();`

---

## REST API

**Q104. What is REST?**
> REST (Representational State Transfer) is an architectural style for designing networked applications. RESTful APIs use standard HTTP methods (GET, POST, PUT, DELETE, PATCH), are stateless, use resource-based URLs, and typically exchange data in JSON or XML format.

---

**Q105. What are HTTP methods used in REST APIs?**
> `GET` retrieves resources, `POST` creates new resources, `PUT` replaces a resource entirely, `PATCH` partially updates a resource, `DELETE` removes a resource. `HEAD` retrieves headers only, and `OPTIONS` describes communication options.

---

**Q106. What are HTTP status codes?**
> - **1xx** Informational
> - **2xx** Success: 200 OK, 201 Created, 204 No Content
> - **3xx** Redirection: 301 Moved Permanently, 302 Found
> - **4xx** Client errors: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable Entity
> - **5xx** Server errors: 500 Internal Server Error, 503 Service Unavailable

---

**Q107. What is CORS?**
> CORS (Cross-Origin Resource Sharing) is a security mechanism that restricts HTTP requests from different origins. Use the `cors` npm package in Express: `app.use(cors())`. Configure allowed origins, methods, and headers to control cross-origin access to your API.

---

**Q108. What is API versioning?**
> API versioning allows maintaining multiple versions of an API simultaneously. Common approaches include URL versioning (`/api/v1/users`), header versioning (`Accept: application/vnd.api.v1+json`), and query string versioning (`?version=1`). URL versioning is most common.

---

**Q109. What is pagination in REST APIs?**
> Pagination limits the amount of data returned in a single response. Common approaches include offset-based (`?page=2&limit=10`), cursor-based (more efficient for large datasets), and keyset pagination. Always include metadata like total count and links to next/prev pages.

---

**Q110. What is request validation in Express?**
> Validate incoming request data to ensure correctness and security. Popular libraries include Joi, `express-validator`, and Zod. Validate `req.body`, `req.params`, and `req.query` before processing. Return 400 Bad Request with descriptive error messages for invalid input.

---

**Q111. What is API documentation?**
> API documentation describes endpoints, parameters, request/response formats, and authentication. Swagger/OpenAPI is the most popular standard. Use `swagger-ui-express` and `swagger-jsdoc` packages to generate interactive API documentation for Express APIs.

---

## Testing

**Q112. What are the types of testing in Node.js?**
> **Unit testing** (testing individual functions/modules in isolation), **integration testing** (testing how modules work together), **end-to-end testing** (testing the full application flow), and **performance/load testing**. Each serves a different purpose in ensuring code quality.

---

**Q113. What is Jest?**
> Jest is a popular JavaScript testing framework developed by Facebook. It provides a test runner, assertion library, mocking capabilities, code coverage reports, and snapshot testing. It works out of the box for both Node.js and React applications.

---

**Q114. What is Mocha?**
> Mocha is a feature-rich JavaScript test framework running on Node.js. It provides the test structure (`describe`, `it`, `before`, `after` hooks) but requires separate assertion and mocking libraries like Chai and Sinon. It is highly configurable and widely used.

---

**Q115. What is Chai?**
> Chai is a BDD/TDD assertion library for Node.js that pairs well with Mocha. It provides three assertion styles: `assert` (traditional), `expect` (BDD), and `should` (BDD). Example: `expect(result).to.equal(42);` or `assert.equal(result, 42);`

---

**Q116. What is Supertest?**
> Supertest is a library for testing HTTP servers. It works well with Express apps and provides a fluent API for making HTTP requests and asserting responses:
> ```js
> await request(app).get('/users').expect(200).expect('Content-Type', /json/);
> ```

---

**Q117. What is mocking in testing?**
> Mocking replaces real dependencies with controlled fake versions during testing. This isolates the code under test from external services, databases, or APIs. Jest has built-in mocking (`jest.mock()`), while Sinon.js is used with Mocha for spies, stubs, and mocks.

---

**Q118. What is code coverage?**
> Code coverage measures the percentage of code executed during tests. Metrics include statement, branch, function, and line coverage. Istanbul (`nyc`) is the most popular coverage tool for Node.js. Jest includes coverage support with the `--coverage` flag.

---

**Q119. What is TDD (Test-Driven Development)?**
> TDD is a development methodology where tests are written before the actual code. The cycle is: write a failing test (Red), write minimum code to pass (Green), then refactor (Refactor). It leads to better design, more maintainable code, and comprehensive test coverage.

---

## Deployment

**Q120. What is PM2?**
> PM2 is a production-grade process manager for Node.js applications. It keeps apps alive forever, reloads without downtime, manages logs, enables cluster mode for multi-core usage, and monitors performance. Install with `npm install -g pm2` and start with `pm2 start app.js`.

---

**Q121. What is Docker and how is it used with Node.js?**
> Docker is a containerization platform that packages applications and their dependencies into containers. For Node.js, create a Dockerfile specifying the Node image, copy files, install dependencies, and define the start command. Use `docker-compose` for multi-service setups.

---

**Q122. What is a Dockerfile for a Node.js app?**
> ```dockerfile
> FROM node:18-alpine
> WORKDIR /app
> COPY package*.json ./
> RUN npm ci --only=production
> COPY . .
> EXPOSE 3000
> CMD ["node", "app.js"]
> ```
> Use alpine images for smaller size and `npm ci` for reproducible installs.

---

**Q123. How do you deploy a Node.js app to Heroku?**
> Initialize a git repo, create a Heroku app with `heroku create`, define a Procfile (`web: node app.js`), ensure you use `process.env.PORT` for the port, and push with `git push heroku main`. Heroku automatically installs dependencies from package.json.

---

**Q124. What is a reverse proxy and why use it with Node.js?**
> A reverse proxy (like Nginx or Apache) sits in front of Node.js and handles SSL termination, load balancing, static file serving, and request routing. Nginx is commonly used to proxy requests to Node.js, handle HTTPS, and serve static assets efficiently.

---

**Q125. What is load balancing?**
> Load balancing distributes incoming requests across multiple server instances to improve performance and reliability. Node.js cluster module enables process-level load balancing. Nginx or cloud load balancers distribute traffic across multiple Node.js server instances.

---

**Q126. What is CI/CD in the context of Node.js?**
> CI/CD (Continuous Integration/Continuous Deployment) automates testing and deployment. Popular tools include GitHub Actions, CircleCI, Jenkins, and GitLab CI. A typical pipeline runs tests, linting, builds Docker images, and deploys to staging/production automatically.

---

**Q127. How do you manage environment variables in production?**
> Use environment variables (never hardcode secrets). In production, set them via the host platform (Heroku config vars, AWS Parameter Store, Kubernetes secrets). Use the `dotenv` package to load `.env` files in non-production environments. Never commit `.env` to version control.

---

## Express.js Advanced

**Q128. What is body-parser in Express?**
> `body-parser` was a third-party middleware to parse HTTP request bodies. Since Express 4.16.0, it is built-in: use `express.json()` for JSON and `express.urlencoded()` for URL-encoded bodies. The standalone `body-parser` package is still used for additional parsers like raw or text.

---

**Q129. What is Morgan?**
> Morgan is an HTTP request logger middleware for Node.js. It logs HTTP requests to the console or a log file. Usage: `app.use(morgan('combined'))` where `'combined'` is the format. Other formats include `'dev'`, `'tiny'`, `'short'`, and custom formats.

---

**Q130. What is compression middleware in Express?**
> The `compression` package provides gzip/deflate compression for HTTP responses. Use `app.use(compression())` to automatically compress response bodies. This significantly reduces bandwidth usage and improves client load times for text-based responses.

---

**Q131. What is cookie-parser middleware?**
> `cookie-parser` parses Cookie header and populates `req.cookies` with an object keyed by cookie names. Use `app.use(cookieParser(secret))` where secret is used to sign cookies (available as `req.signedCookies`). Unsigned cookies are in `req.cookies`.

---

**Q132. What is multer?**
> Multer is a middleware for handling `multipart/form-data`, primarily used for file uploads. Configure it with storage options (disk or memory), file size limits, and file filter. Files are available as `req.file` (single) or `req.files` (multiple) after processing.

---

**Q133. What is the difference between PUT and PATCH?**
> `PUT` replaces the entire resource with the provided data — fields not included in the request are removed. `PATCH` applies partial modifications — only the provided fields are updated. PATCH is more efficient for partial updates and is idempotent like PUT.

---

**Q134. How do you implement file upload in Express?**
> Use the multer middleware:
> ```js
> const upload = multer({ dest: 'uploads/' });
> app.post('/upload', upload.single('file'), (req, res) => {
>   console.log(req.file);
>   res.send('Uploaded');
> });
> ```

---

**Q135. What is express-validator?**
> `express-validator` is a set of Express middlewares wrapping the validator.js library. It provides chain-based validation and sanitization. Use `body()`, `param()`, `query()` methods to validate fields, then `validationResult()` to collect errors.

---

**Q136. How do you implement request logging in Express?**
> Use Morgan for HTTP request logging: `app.use(morgan('dev'))`. For application-level logging, use `winston` or `pino` logger libraries which support log levels, transports (file, console, cloud), and structured logging in JSON format.

---

**Q137. What is express-async-errors?**
> `express-async-errors` patches Express to automatically catch errors thrown in async route handlers and pass them to error-handling middleware. Without it, you need to manually wrap async handlers in try/catch or use wrapper functions.

---

**Q138. What is CSRF protection in Express?**
> CSRF (Cross-Site Request Forgery) protection can be implemented using the `csurf` middleware (deprecated in newer versions). Modern alternatives include the `SameSite` cookie attribute, double-submit cookie pattern, or custom CSRF tokens for state-changing operations.

---

**Q139. What is express-slow-down?**
> `express-slow-down` is middleware that slows down responses rather than blocking them after a number of requests. It adds a delay to each request after the limit is reached. It can be used alongside `express-rate-limit` for a gentler approach to rate limiting.

---

**Q140. How do you implement caching in Express?**
> Options include: setting `Cache-Control` headers (`res.set('Cache-Control', 'public, max-age=3600')`), using Redis for server-side caching, using `memory-cache` or `node-cache` packages, or using a CDN for static assets.

---

**Q141. What is the apicache middleware?**
> `apicache` is middleware for simple API response caching in Express. It caches responses for a specified duration. Usage: `app.use('/api', apicache.middleware('5 minutes'))`. It reduces database load for frequently accessed, rarely changing endpoints.

---

**Q142. How do you handle WebSockets with Express?**
> Use the `ws` package alongside the existing HTTP server:
> ```js
> const server = http.createServer(app);
> const wss = new WebSocket.Server({ server });
> wss.on('connection', (ws) => { ws.on('message', (msg) => console.log(msg)); });
> ```

---

**Q143. What is socket.io?**
> Socket.io is a library enabling real-time, bidirectional communication between web clients and servers. It uses WebSocket when available, with fallbacks to long polling. It supports rooms, namespaces, broadcasting, and acknowledgments. It works well alongside Express.

---

**Q144. What is the http-errors package?**
> `http-errors` creates HTTP error objects with status codes and messages. Example: `throw createError(404, 'User not found')`. These errors can be caught by Express error-handling middleware and formatted into appropriate HTTP responses.

---

**Q145. How do you implement search functionality in an Express API?**
> Accept search parameters via query string (`req.query.q`), sanitize input to prevent injection, build database queries using LIKE (SQL) or `$text`/`$regex` (MongoDB), and return paginated results. Consider full-text search solutions like Elasticsearch for complex needs.

---

**Q146. What is the purpose of app.set() and app.get() for configuration?**
> `app.set(name, value)` sets application settings like `'view engine'`, `'views'`, `'env'`, `'port'`, or `'trust proxy'`. `app.get(name)` retrieves the setting value. These are different from route-handling — they share the same method name but serve different purposes.

---

**Q147. What is the trust proxy setting in Express?**
> The `trust proxy` setting tells Express to trust the `X-Forwarded-For` header from proxies/load balancers to get the real client IP. Set with `app.set('trust proxy', 1)` when behind a single proxy. Without it, `req.ip` returns the proxy IP, not the actual client IP.

---

**Q148. How do you implement content negotiation in Express?**
> Use `res.format()` to respond with different content types based on the request's `Accept` header:
> ```js
> res.format({
>   'text/html': () => res.send('<p>HTML</p>'),
>   'application/json': () => res.json({ msg: 'JSON' })
> });
> ```

---

**Q149. How do you send a file as a download in Express?**
> Use `res.download(path, [filename], [callback])`. It sets the `Content-Disposition` header to `'attachment'`, prompting the browser to download the file. The optional filename parameter sets the downloaded file's name. `res.sendFile()` serves files inline (displayed in browser).

---

**Q150. What is sub-app mounting in Express?**
> Express allows mounting one application (sub-app) into another: `parentApp.use('/admin', adminApp)`. The sub-app is a full Express app with its own middleware, routes, and settings. Settings are not inherited from parent except for `'trust proxy'` and `'views'`.

---

**Q151. How do you implement server-sent events (SSE) in Express?**
> Set headers for SSE, then write data:
> ```js
> res.setHeader('Content-Type', 'text/event-stream');
> res.setHeader('Cache-Control', 'no-cache');
> res.setHeader('Connection', 'keep-alive');
> res.write('data: ' + JSON.stringify(data) + '\n\n');
> ```
> SSE provides one-way real-time updates from server to client.

---

**Q152. What is the accepts() method in Express?**
> `req.accepts(types)` checks if the specified content types are acceptable based on the request's `Accept` header. Returns the best match or `false`. Example: `req.accepts(['html', 'json'])` returns `'json'` if the client prefers JSON. Used for content negotiation.

---

**Q153. What is the purpose of app.engine() in Express?**
> `app.engine(ext, callback)` registers a template engine callback for a file extension. This allows using template engines that don't follow the standard Express signature, or registering the same engine for multiple extensions. Example: `app.engine('html', require('ejs').renderFile)`.

---

## Node.js Advanced

**Q154. What is the util module in Node.js?**
> The `util` module provides utility functions for Node.js. Key functions include `util.promisify()` (converts callback-based functions to Promise-based), `util.inspect()` (detailed object inspection), `util.format()` (string formatting), and `util.inherits()` (prototype inheritance).

---

**Q155. What is util.promisify()?**
> `util.promisify()` converts a function following the Node.js error-first callback convention to one that returns a Promise. Example:
> ```js
> const readFile = util.promisify(fs.readFile);
> const data = await readFile('file.txt', 'utf8');
> ```

---

**Q156. What is the cluster module?**
> The cluster module allows a Node.js process to spawn child worker processes that share server ports. The master process manages workers and can restart crashed ones. It effectively utilizes multi-core processors since each worker runs on its own core/thread.

---

**Q157. What is the vm module in Node.js?**
> The `vm` module allows compiling and running code within V8 Virtual Machine contexts. It provides `vm.runInNewContext()`, `vm.runInContext()`, and `vm.Script` class. It is useful for executing untrusted code in isolated contexts, though it is not a complete security sandbox.

---

**Q158. What is the readline module?**
> The `readline` module provides an interface for reading data from a Readable stream line by line. It is commonly used for creating command-line interfaces or processing large text files. Use `readline.createInterface()` with an input stream.

---

**Q159. What are generators in Node.js?**
> Generators are functions that can be paused and resumed using `yield`. Defined with `function*`, they return a generator iterator. They were used for async flow control before async/await, and are still useful for creating lazy iterators or infinite sequences.

---

**Q160. What is the assert module?**
> The `assert` module provides assertion functions for testing invariants. Functions include `assert.equal()`, `assert.deepEqual()`, `assert.strictEqual()`, `assert.throws()`, and `assert.rejects()`. It is used in unit tests and to validate assumptions in code.

---

**Q161. What is Node.js REPL?**
> REPL (Read-Eval-Print Loop) is an interactive shell for Node.js accessible by typing `node` in the terminal. It reads input, evaluates JavaScript expressions, prints results, and loops. It is useful for experimenting, debugging, and learning Node.js.

---

**Q162. What is the zlib module?**
> The `zlib` module provides compression functionality using Gzip, Deflate, and Brotli algorithms. It supports both synchronous and streaming compression/decompression. Used for compressing HTTP responses, files, and data in network applications.

---

**Q163. What is the dns module?**
> The `dns` module provides DNS lookup and reverse lookup capabilities. `dns.lookup()` resolves hostnames to IP addresses using the OS resolver. `dns.resolve()` functions use the network directly. It is used when you need to validate or resolve domain names.

---

**Q164. What is the tls module?**
> The `tls` module implements TLS (Transport Layer Security) and SSL protocols on top of the `net` module. It is used to create encrypted connections. The `https` module uses `tls` internally for secure HTTP communication.

---

**Q165. What are Proxy objects in Node.js?**
> Proxy is a built-in JavaScript feature that wraps an object and intercepts fundamental operations (get, set, has, delete, etc.) via handler traps. Proxies enable meta-programming, validation, logging, and reactive patterns.

---

**Q166. What is WeakMap and WeakSet in Node.js?**
> `WeakMap` stores key-value pairs where keys must be objects and are weakly referenced (can be garbage collected if no other references exist). `WeakSet` is a collection of objects with weak references. Both are useful for caching and avoiding memory leaks.

---

**Q167. What is the difference between synchronous and asynchronous file operations in Node.js?**
> Synchronous operations (e.g., `fs.readFileSync`) block the event loop until complete, freezing the application. Asynchronous operations (e.g., `fs.readFile`) are non-blocking and use callbacks, Promises, or async/await. Always use async operations in production for scalability.

---

**Q168. What is the AbortController in Node.js?**
> `AbortController` is a Web API available in Node.js 15+ for aborting asynchronous operations. Create a controller, pass its signal to `fetch` or other async operations, and call `controller.abort()` to cancel them. Useful for implementing request timeouts.

---

**Q169. What is the difference between setTimeout(fn, 0) and process.nextTick()?**
> `process.nextTick()` fires before any I/O events in the current iteration of the event loop, before `setTimeout`. `setTimeout(fn, 0)` fires in the timers phase of the next event loop iteration. Use `nextTick` for code that must run after the current operation but before I/O.

---

**Q170. What are async iterators in Node.js?**
> Async iterators allow iterating over asynchronous data sources using `for await...of` loops. Readable streams implement the async iterator protocol in Node.js 10+. Custom async iterables implement `Symbol.asyncIterator` returning an object with an async `next()` method.

---

## Microservices & Architecture

**Q171. What is microservices architecture?**
> Microservices is an architectural style where an application is built as a collection of small, independently deployable services. Each service handles a specific business function, has its own database, and communicates via APIs or message queues. Node.js is well-suited for microservices.

---

**Q172. What is message queue and why use it?**
> Message queues (like RabbitMQ, Apache Kafka, or Amazon SQS) enable asynchronous communication between services. They decouple producers from consumers, provide buffering, and improve resilience. Use `amqplib` for RabbitMQ or `kafkajs` for Kafka in Node.js.

---

**Q173. What is GraphQL and how does it compare to REST?**
> GraphQL is a query language for APIs where clients specify exactly what data they need. Unlike REST's multiple fixed endpoints, GraphQL has a single endpoint. It solves over-fetching and under-fetching problems. Use `apollo-server` or `graphql-yoga` with Express.

---

**Q174. What is an API Gateway?**
> An API Gateway is a server that acts as the entry point for all client requests in a microservices architecture. It handles routing, authentication, rate limiting, SSL termination, request/response transformation, and load balancing. Examples: Kong, AWS API Gateway, and Nginx.

---

**Q175. What is gRPC and when to use it in Node.js?**
> gRPC is a high-performance RPC framework using Protocol Buffers for serialization. It is faster and more efficient than REST for inter-service communication. Use the `@grpc/grpc-js` package for Node.js. Best for internal microservice communication where performance is critical.

---

## Logging & Monitoring

**Q176. What is Winston?**
> Winston is a versatile logging library for Node.js. It supports multiple log levels (error, warn, info, debug), multiple transports (console, file, database, cloud services), custom formatting, and log rotation. It is the most widely used logging library in the Node.js ecosystem.

---

**Q177. What is Pino?**
> Pino is a very low overhead Node.js logger focused on performance. It outputs JSON logs and is designed to be extremely fast. It supports log levels, child loggers, and multiple transports. It is especially popular in performance-critical applications.

---

**Q178. What is application monitoring?**
> Application monitoring tracks performance, errors, and availability of your Node.js application in real-time. Tools include New Relic, Datadog, Dynatrace, AppDynamics, and open-source options like Prometheus with Grafana. Node.js-specific tools include clinic.js.

---

**Q179. What is the debug package?**
> The `debug` package is a small debugging utility. Set the `DEBUG` environment variable to enable specific debug namespaces. Example: `DEBUG=myapp:* node app.js` will show all debug messages in the `myapp` namespace.

---

**Q180. What is Prometheus and how to use it with Node.js?**
> Prometheus is an open-source monitoring and alerting toolkit. Use the `prom-client` package to expose metrics (counters, gauges, histograms) from your Node.js app at a `/metrics` endpoint. Prometheus scrapes these metrics and Grafana visualizes them.

---

## Miscellaneous

**Q181. What is nvm?**
> nvm (Node Version Manager) allows installing and switching between multiple versions of Node.js on the same machine. Commands include `nvm install 18`, `nvm use 18`, `nvm alias default 18`. It is essential for working on multiple projects requiring different Node.js versions.

---

**Q182. What is npx?**
> npx is a package runner that comes with npm 5.2+. It allows executing npm packages without installing them globally. It downloads the package temporarily, runs it, then removes it. Common use: `npx create-react-app my-app`, `npx nodemon app.js`.

---

**Q183. What is the .nvmrc file?**
> `.nvmrc` is a file in a project's root directory specifying the Node.js version to use. Running `nvm use` in a directory with `.nvmrc` automatically switches to the specified version. This ensures all team members use the same Node.js version.

---

**Q184. What is TypeScript with Node.js?**
> TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. With Node.js, use `ts-node` for direct execution or `tsc` to compile. TypeScript adds static typing, interfaces, enums, and decorators, catching errors at compile time rather than runtime.

---

**Q185. What is nodemon?**
> nodemon is a utility that automatically restarts a Node.js application when file changes are detected. It is used during development: `nodemon app.js`. Configure it via `nodemon.json` or `package.json` to watch specific files or directories.

---

**Q186. What is the difference between LTS and Current Node.js versions?**
> LTS (Long Term Support) versions receive critical bug fixes and security patches for 30 months and are recommended for production use. Current versions have the latest features but shorter support windows. LTS versions have even major version numbers (18, 20, 22).

---

**Q187. What is yarn?**
> Yarn is an alternative package manager to npm developed by Facebook. It offers faster installs, deterministic dependency resolution (via `yarn.lock`), workspaces for monorepos, and offline mode. Yarn Berry (v2+) introduced Plug'n'Play for zero-installs.

---

**Q188. What is pnpm?**
> pnpm is a fast, disk-efficient package manager for Node.js. It uses a content-addressable store and hard links, so packages are installed once and shared across projects. It is significantly faster than npm and yarn for large monorepos.

---

**Q189. What is a monorepo?**
> A monorepo is a single repository containing multiple packages or applications. Tools like Lerna, Nx, Turborepo, and npm/yarn/pnpm workspaces help manage monorepos. They simplify code sharing, atomic commits across packages, and unified tooling.

---

**Q190. What is ESLint?**
> ESLint is a static code analysis tool for identifying problematic patterns in JavaScript/Node.js code. Configure rules in `.eslintrc`, integrate with editors and CI pipelines. It supports plugins (`eslint-plugin-node`, `eslint-config-airbnb`) and auto-fixes many issues.

---

**Q191. What is Prettier?**
> Prettier is an opinionated code formatter that automatically formats code to a consistent style. Unlike linters that find bugs, Prettier focuses purely on code formatting. Configure via `.prettierrc`. Often used alongside ESLint: Prettier handles formatting, ESLint handles code quality.

---

**Q192. What is the node_modules directory?**
> `node_modules` is the directory where npm/yarn/pnpm installs project dependencies. It should be added to `.gitignore` and not committed to version control. Dependencies can be reinstalled from `package.json` using `npm install`.

---

**Q193. What is the difference between npm install and npm ci?**
> `npm install` reads `package.json` and may update `package-lock.json`. `npm ci` (clean install) strictly installs from `package-lock.json` without modifying it and deletes `node_modules` first. `npm ci` is faster and more reliable for CI/CD pipelines and production deployments.

---

**Q194. What is package-lock.json?**
> `package-lock.json` is automatically generated by npm and locks the exact versions of all installed dependencies (including transitive dependencies). It ensures everyone on the team installs the same dependency versions. Always commit it to version control.

---

**Q195. What is semantic versioning (semver)?**
> Semver is a versioning scheme with three numbers: `MAJOR.MINOR.PATCH`. MAJOR: breaking changes. MINOR: new features (backward compatible). PATCH: bug fixes (backward compatible). In package.json, `^` allows minor/patch updates, `~` allows only patch updates.

---

**Q196. What is the purpose of .gitignore in a Node.js project?**
> `.gitignore` specifies files and directories that Git should not track. For Node.js, always include: `node_modules/`, `.env`, `dist/`, `build/`, `coverage/`, `*.log`, and any files with secrets. Use gitignore.io to generate templates.

---

**Q197. How do you run scripts in package.json?**
> Define scripts in the `"scripts"` section of package.json and run them with `npm run <script-name>`. Lifecycle scripts like `start`, `test`, and `build` have shortcuts (`npm start`, `npm test`). Pre/post hooks are supported (`pretest` runs before `npm test`).

---

**Q198. What is the Inspector API in Node.js?**
> The `inspector` module provides an interface to the V8 inspector. It enables programmatic debugging, CPU profiling, and heap snapshots. Use `inspector.open()` to start the inspector and connect from Chrome DevTools or VS Code debugger.

---

**Q199. What is a memory heap in Node.js?**
> The memory heap is where dynamic memory allocation happens in Node.js (for objects and closures). V8 manages the heap and performs garbage collection. Monitor heap usage with `process.memoryUsage()`. Large heaps or leaks can be diagnosed with heap snapshots in Chrome DevTools.

---

**Q200. What is the express-async-handler package?**
> `express-async-handler` is a simple middleware wrapper for async Express route handlers that automatically passes rejected Promise errors to `next()`. Usage:
> ```js
> router.get('/', asyncHandler(async (req, res) => {
>   const data = await getData();
>   res.json(data);
> }));
> ```

---

**Q201. How do you implement API key authentication in Express?**
> Create middleware that checks the request headers for an API key:
> ```js
> app.use((req, res, next) => {
>   if (req.headers['x-api-key'] === process.env.API_KEY) next();
>   else res.status(401).json({ error: 'Invalid API key' });
> });
> ```

---

**Q202. What is the vary header and how does it work in Express?**
> The `Vary` header tells caches that the response varies based on specific request headers. Express's `res.vary()` adds to the Vary header. The `compression` middleware automatically adds `'Accept-Encoding'` to Vary. Important for content negotiation and caching correctness.

---

**Q203. What is the difference between app-level and router-level error handling?**
> App-level error handlers (`app.use` with 4 params) catch errors from all routes. Router-level error handlers (`router.use` with 4 params) only catch errors from routes on that router. Router-level errors propagate to app-level if not handled in the router.

---

**Q204. What are Symbol types in Node.js?**
> `Symbol` is a primitive data type in JavaScript (ES6) that creates unique identifiers. In Node.js, Symbols are used as unique property keys. Well-known symbols (`Symbol.iterator`, `Symbol.asyncIterator`) define standard behaviors for objects.

---

**Q205. What is the difference between fork() and spawn() in child_process?**
> `fork()` is a special case of `spawn()` specifically for spawning new Node.js processes. It establishes a communication channel (IPC) between the parent and child process via `process.send()` and `process.on('message')`. `spawn()` is for any external command without the IPC channel.

---

*Total: 205 Questions | Covering Node.js & Express.js from Basics to Advanced*
