events-lib
----------

tiny javascript-amd library for common usage

you need [require.js](http://requirejs.org/) first.

basic usage
-----------

```javascript
require (['event', 'event-emitter', 'signal'], function (event, eventEmitter, signal) {

  var e = new event ();
  // Event is just an observable
  var listener = console.log.bind (console, 'listener'); // es5 hint
  e.addListener    (listener);
  e.dispatch       (1,2,3); // -> "listener" 1 2 3
  e.dispatchArray  (4,5,6); // -> "listener" 1 2 3
  e.removeListener (listener);


  var ee = new eventEmitter ();
  // Event emitter more like pubsub
  ee.addEventListener   ('event', console.log.bind (console, 'listener 2'));
  ee.dispatchEvent      ('event', 7, 8);  // -> "listener 2" 7 8
  ee.dispatchEventArray ('event', 9, 10); // -> "listener 2" 9 10


  var s = new signal ();
  // Signal is like event, but fires only once
  s.addListener (console.log.bind (console, 'listner 3'));
  s.dispatch (11, 12);  // -> "listener 3" 11 12
  s.dispatch (13, 14);  // nothing  

});
```

advanced usage
--------------

actually this library intended to be base for more complicated objects, so feel free to do something like this:

```javascript
function jsLoader () {
  this.ready = new signal ();
  this.error = new signal ();
}
jsLoader.prototype.load = function () {
  var self = this;
  someAsyncFunction (function (err, result){
    if (err) {
      self.error.dispatch (err);
    } else {
      self.ready.dispatch (result);
    }
  });
}
```