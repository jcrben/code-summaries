
#### Introduction
Motivated by mysterious failures with no errors, I dove into the code:
  * [`req = requirejs = function (...)`](https://github.com/requirejs/requirejs/blob/2.1.20/require.js#L1732) is the main entry point
    * about 75 lines later this function is called with an empty object to create a "default context" of `_`
    * This is also where `configure()` happens
  * [`define = function (name, deps, callback)`](https://github.com/requirejs/requirejs/blob/2.1.20/require.js#L2019) is the next major path. It takes a bunch a module names and pushes them into `context.defQueue` with a companion `context.defQueueMap[name]`
  * at some point (TODO: figure out where), context.defQueue() items are loaded mainly through the [`Module() constructor function`](https://github.com/requirejs/requirejs/blob/2.1.20/require.js#L722)
    * [`getModule()`](https://github.com/requirejs/requirejs/blob/2.1.20/require.js#L499) and its load function.
    * If you follow `Module.load()`, you will eventually get to [`req.load = function(...)`](https://github.com/requirejs/requirejs/blob/2.1.20/require.js#L1862) which will create script tags for the browser loading case.

#### Size in lines of code
[TODO]

#### See also
* [How does require.js work? How it load files? Does it make ajax call to load files or any other way? Can anyone explain it clearly?](https://www.quora.com/How-does-require-js-work-How-it-load-files-Does-it-make-ajax-call-to-load-files-or-any-other-way-Can-anyone-explain-it-clearly)
