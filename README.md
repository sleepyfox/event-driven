# Event-driven asynchronicity

In JavaScript, there are two (or three, depending on how you count) ways of dealing with asynchronous code, callbacks and promises. Async/await is really just syntactic sugar around promises, but you may call that a third way if you wish.

All of these methods suffer problems:

* Callbacks: Xmas tree' style deep code nesting;
* Promises: complex .then .catch .finally chaining;
* Promises: clunky ways of dealing with multiple promises e.g. Promise.allSettled, which has a different API than normal Promises;
* Async/await: the need to surround any use of `await` with a try/catch if the called code may reject;
* Async/await: the 'infestation' of code as any function that awaits must be async, and any code that awaits that must be async, and...

Another possibility, that few experiment with, is to use [event emitters](https://nodejs.org/api/events.html). This style of programming is common in the browser, but I have never seen anyone use it on the back-end. The principle is simple:

* Events are published ('emitted');
* Events have data;
* Events can be subscribed to;
* Subscribing functions are invoked with the event data, after the event is published.

This allows us to separate (spatially) the caller and called, the asynchronous nature of which is what causes complication in code structure with callbacks or promises, as the caller and the called need to be causally linked in the same textual location in the code.

## Kata

The nature of the kata is a simple task: get some data from a web service and write it to disk after having processed the data.

1. Get data blog post data from web service
1. Create a report calculating summary data
1. Write the summary report to disk

The first and last steps are, by nature, async. Although the middle task is synchronous, it benefits from being treated asyncronously, as in time with a large enough volume of data the calculation of the report may become quite compute-intensive.

A naive implementation of this system will look like the starter code present in [this refactoring kata](https://github.com/sleepyfox/fcis). 

The objective of _this_kata is to create a system that accomplishes the same aims by implementing the event-emmitter pattern.
