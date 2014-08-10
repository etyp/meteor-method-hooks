# Meteor Method Hooks

### Provides before/after hooks for Meteor methods

This server-only package extends Meteor with two methods: `beforeMethod` and `afterMethod`. Potental uses include:

* Security
* Logging
* Making your wildest dreams come true

Here's an example for security, in `/server/test.coffee`

```
Meteor.beforeMethod 'test', ->
  Meteor.users.findOne(@userId)?.admin
```

The above will prevent the `test` method from being executed unless the client is logged in as and has their `admin` field set to true. Any `beforeMethod` that returns `false` will halt the method and any other hooks from executing.

You can pass an array of method names, and the hooks will recieve the same parameters as the original method. For example:

```
if Meteor.isServer
  Meteor.methods
    'test1' : (str) -> console.log 'Hi', str
    'test2' : -> console.log 'second method'

  Meteor.beforeMethod ['test1','test2'], (str) ->
    console.log 'hook1', str

  Meteor.beforeMethod 'test1', ->
    console.log 'hook2'

if Meteor.isClient
  Meteor.call 'test1', 'Chris'
  Meteor.call 'test2'
```

will output

```
hook1 Chris
hook2
Hi Chris
hook1 undefined
second method
hook3
```

## TODO

* Testing
* `beforeAllMethods` & `afterAllMethods` ?

## Credits

[Chris Hitchcott](http://github.com/hitchcott), 2014