<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Size in lines of code](#size-in-lines-of-code)
- [Basic observations](#basic-observations)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

As of July 4, 2015, based on [commit 6c7a36733891ddd6089ee4f267d731383bf58ea9](https://github.com/strongloop/express/commit/6c7a36733891ddd6089ee4f267d731383bf58ea9). All links to source should be as of this date as well.

#### Size in lines of code
Size of the core code using https://github.com/flosse/sloc. Not that much!


    ┌─────────────────────────┬──────────┬────────┬─────────┐
    │ Path                    │ Physical │ Source │ Comment │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/application.js      │ 643      │ 268    │ 271     │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/express.js          │ 103      │ 55     │ 32      │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/request.js          │ 489      │ 151    │ 273     │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/response.js         │ 1053     │ 464    │ 448     │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/view.js             │ 173      │ 71     │ 67      │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/utils.js            │ 286      │ 126    │ 113     │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/middleware/init.js  │ 36       │ 13     │ 16      │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/middleware/query.js │ 40       │ 17     │ 15      │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/router/index.js     │ 642      │ 367    │ 156     │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/router/layer.js     │ 176      │ 87     │ 56      │
    ├─────────────────────────┼──────────┼────────┼─────────┤
    │ lib/router/route.js     │ 210      │ 99     │ 67      │
    └─────────────────────────┴──────────┴────────┴─────────┘

#### Initialization and first observations
Let's walk through the [hello world example](http://expressjs.com/starter/hello-world.html).
`var app = express()` returns an app defined in `lib/express.js` which is a basic function calling `app.handle`, defined in `lib/application.js` ([source](https://github.com/strongloop/express/blob/6c7a36733891ddd6089ee4f267d731383bf58ea9/lib/application.js#L157)) with additional functionality "mixed in" using the merge-desciptor package, which relies upon [Object.getOwnPropertyDescriptor()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor). [EventEmitter.prototype](https://nodejs.org/api/events.html) is added, then `lib/application.js` (named "proto") adds a bunch of functions. Next, `app.request` and `app.response` properties are defined such that their `__proto__` is `lib/request` and `lib/response`, respectively.

`app.init()` mainly runs the defaultConfiguration function inside `lib/application.js` ([source](https://github.com/strongloop/express/blob/e71014f522fb8db9ba1ef634f2e9f2c8069e4b6c/lib/application.js#L70)), which populates settings using the `set` method also defined in that file and creates the `on('mount')` event reaction.

At this point the app is defined and returned. The next step is to create a server.  `app.listen` simply calls the node.js function `http.createServer`, which creates an instance of `http.Server`. This server's prototype is [`net.Server`](https://nodejs.org/api/net.html#net_class_net_server) from the `net` module, which is where the `address()` method is from.
