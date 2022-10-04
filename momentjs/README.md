[Moment.js] module bundle (see [jenkins-js-modules]).

> __Note__: __DEPRECATED__
> This JavaScript library [can now be easily externalized via the package.json](https://github.com/jenkinsci/js-samples/blob/master/step-04-externalize-libs/HOW-IT-WORKS.md#configure-node-build-to-externalize-dependencies). 

`import` this bundle (see [jenkins-js-modules]) into your application bundle (in your plugin etc) instead of bundling
[Moment.js] in your application bundle, making your app bundle considerably lighter.

# HPI Dependency
Your plugin needs to add a dependency on this plugin (to ensure it gets installed in Jenkins). 

```xml
<artifactItem>
    <groupId>org.jenkins-ci.ui</groupId>
    <artifactId>momentjs</artifactId>
    <version>[VERSION]</version>
</artifactItem>
```

> See _[wiki.jenkins-ci.org](https://wiki.jenkins-ci.org/display/JENKINS/Moment.js)_ to get the latest version.

# Using Moment.js v2:

* __Bundle Id__: `momentjs:momentjs2`

There are 2 ways of using `momentjs:momentjs2` in your app bundle:
 
1. Normal `require` syntax (synchronous) on the standard `moment` NPM module, and then using a `withExternalModuleMapping` instruction ([jenkins-js-builder]) in the app bundle's `gulpfile.js`.  
1. Lower level `import` syntax (asynchronous).
  
## `require` (sync)
If using [jenkins-js-builder] to create yor application bundle, you can code your application's CommonJS modules to
use the more simple CommonJS style `require` syntax (synch), as opposed to the lower level `import` syntax (async)
of [jenkins-js-modules].
   
When doing it this way, your module code should require the `moment` NPM module and use it as normal e.g.

```javascript
var moment = require('moment');

moment().format('MMMM Do YYYY, h:mm:ss a');
```
    
> __Note__: You should install `moment` as a dev dependency i.e. `npm install --save-dev moment`
    
The above code will work fine as is, but the only downside is that your app bundle will be very bloated as it will
include the `moment` NPM module. To lighten your bundle for the browser (by using a shared instance of the `moment`
NPM module), use [jenkins-js-builder] to create your app bundle (in your `gulpfile.js`), telling it to "map" (transform) all
synchronous `require` calls for `moment` to async `import`<sub>s</sub> of the `momentjs:momentjs2`
bundle (which actually `export`<sub>s</sub> `moment`) e.g.

```javascript
var builder = require('jenkins-js-builder');

//
// Use the predefined tasks from jenkins-js-builder.
//
builder.defineTasks(['test', 'bundle']);

//
// Create the app bundle, mapping sync require calls for 'moment' to 
// async imports of 'momentjs:momentjs2'.
//
builder.bundle('src/main/js/myapp.js')
    .withExternalModuleMapping('moment', 'momentjs:momentjs2')
    .inDir('src/main/webapp/bundles');
```
    
All of the above "magically" translates the appropriate bits of your app bundle's JS code to use async `import` calls
(see below) in a way that just works.     

## `import` (async)  
You can also use the lower level asynchronous `import` call ([jenkins-js-modules]) to get your `moment` reference.  

```javascript
require('jenkins-js-modules')
    .import('momentjs:momentjs2')
    .onFulfilled(function(moment) {
        moment().format('MMMM Do YYYY, h:mm:ss a');
    });
```

> __Note__: Using this async `import` approach makes unit testing of your JavaScript modules more tricky because 
> your test scaffolding code will need to manually `export` the `moment` module as `momentjs:momentjs2`
> in order for the subsequent `import` to work without failure. This is not an issue when using the synchronous `require`
> approach (see above) because the bundle `import` is only introduced to the JS code as the bundle is being created.

[Moment.js]: http://momentjs.com/
[jenkins-js-builder]: https://github.com/tfennelly/jenkins-js-builder
[jenkins-js-modules]: https://github.com/tfennelly/jenkins-js-modules