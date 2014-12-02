# tiger8-commonjs [![Build Status](https://travis-ci.org/tiger8/tiger8-commonjs.svg?branch=master&style=flat)](https://travis-ci.org/tiger8/tiger8-commonjs) [![Appcelerator Titanium](http://www-static.appcelerator.com/badges/alloy-git-badge-sq.png)](http://www.appcelerator.com/titanium/alloy/) 


<img align="right" width="200" height="200" src="https://avatars0.githubusercontent.com/u/9886051?v=3&s=200">

> **ALPHA -- Here be dragons...** 

**OK.  Here's the deal. ** [Tony Lukasavage](https://github.com/tonylukasavage "Tony Lukasavage") wrote an awesome commonjs module, [ti-commonjs](https://github.com/tonylukasavage/ti-commonjs "ti-commonjs"), for implementing commonjs inside an Appcelerator Titanium application.    There are only two changes to his module:  

1.  The module is split into two different modules:  
	- [genesis-commonjs](https://github.com/tiger8/genesis-commonjs "genesis-commonjs") -- A [ti-genesis](https://github.com/tiger8/genesis "ti-genesis") module for executing compile time tasks that need to happen
	- [tiger8-commonjs](https://github.com/tiger8/tiger8-commonjs "tiger8-commonjs") (this module) -- A [tiger8](https://github.com/tiger8/tiger8 "tiger8") module with the code used at runtime
2.	Some changes were made to the runtime code to get it to work with mobile web projects. 

**All credit** for this goes to [Tony Lukasavage](https://github.com/tonylukasavage "Tony Lukasavage").   We will try to integrate any changes made to his module into this module to keep it awesome!

## Description 

Node.js-style CommonJS in Appcelerator Titanium via Alloy. For full details on what exactly this means, check out Node.js's own [documentation on its CommonJS implementation](http://nodejs.org/api/modules.html). In addition to this added functionality, `ti-commonjs` also eliminates _all_ platform-specific disparities in Titanium's CommonJS implementation.

## Requirements ![](http://img.shields.io/badge/titanium-3.2+-green.svg?style=flat) ![](http://img.shields.io/badge/alloy-1.3+-green.svg?style=flat)

* Titanium ➜ `npm install -g titanium` ➜ `ti sdk install`
* Alloy ➜ `npm install -g alloy`

It is distinctly possible that `tiger8-commonjs` will work with earlier versions of both Titanium and Alloy, but they are untested and unsupported.

## Supports

* Android emulator and device
* iOS simulator and device
* Mobile Web
* 
## Install [![NPM version](https://badge.fury.io/js/tiger8-commonjs.svg?style=flat)](http://badge.fury.io/js/tiger8-commonjs)

*Assuming you're in your Alloy project's `app/lib` folder*

```bash
$ npm install tiger8-commonjs
```
[![NPM](https://nodei.co/npm/tiger8-commonjs.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/tiger8-commonjs/)

Aside from installing `tiger8-commonjs`, this will also add the `alloy.jmk` file (or modifications to existing alloy.jmk) necessary to post-process your generated runtime files. Read [here](http://docs.appcelerator.com/titanium/latest/#!/guide/Build_Configuration_File_(alloy.jmk)) for more details on alloy.jmk files.


## Testing [![Built with Grunt](https://cdn.gruntjs.com/builtwith.png)](http://gruntjs.com/)

```bash
# run jshint and unit tests
$ grunt

# create coverage report in ./coverage/index.html
$ grunt coverage
```

## Usage

### absolute paths (relative to "Resources" folder)

```javascript
require('/foo/bar');
```

### relative paths

```javascript
require('../../someModule');
require('./someModuleInCurrentFolder');
require('.././some/ridiculous/../../path');
```

### loading from `node_modules` folder

Load modules installed via npm in `Resources/node_modules`. Full details [here](http://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders).

```javascript
var _ = require('underscore');
require('ti-mocha');
```

You can also target specific files within those npm installations. For example, let's take [should.js](https://github.com/visionmedia/should.js/). While you can't require it directly in Titanium, due to its reliance on node.js libraries, you can reference the single-file Titanium-compatible version embedded in the installation. This tactic works with almost every npm distributed module that has a browser-compatible version.

```js
var should = require('should'); // ERROR - missing node.js libraries

var should = require('should/should'); // WOOHOO! It works in Titanium.
```

### folders as modules

If a folder contains a `package.json`, the path in its `main` property will be loaded as a module. Additionally, a module named `index.js` can be referenced just by its folder's name. The following example demonstrates both uses. Full details [here](http://nodejs.org/api/modules.html#modules_folders_as_modules).

#### /foo/package.json
```json
{
	"main": "./lib/quux"
}
```

#### /app.js
```javascript
// assuming the module "Resources/foo/lib/quux.js" exists...
var foo = require('/foo');

// assuming the module "Resources/bar/index.js" exists...
var bar = require('/bar');
```

### loading JSON files

You can now load JSON files simply with `require()`.

```js
console.log(require('/package.json').main);

// prints "./lib/ti-commonjs.js"
```

### require.resolve()

Get the full path resolved path to a module.

```js
require.resolve('/foo/bar.js') === require.resolve('/.././foo/../foo/bar');
```

### require.main

Every require function now has a reference to the main module (app.js). Full details [here](http://nodejs.org/api/modules.html#modules_accessing_the_main_module).

#### /foo/bar.js
```js
require.main.id === '.' && require.main.filename === '/app.js'
```

### load module with or without extensions

```js
require('/foo') === require('/foo.js')
```

### "module" object

Titanium's implementation gives limited access to the properties of the `module` object. With `ti-commonjs.js` you have full access to the following properties and functions. Full details [here](http://nodejs.org/api/modules.html#modules_the_module_object).

* [module.exports](http://nodejs.org/api/modules.html#modules_module_exports)
* [exports](http://nodejs.org/api/modules.html#modules_exports_alias)
* [module.require(id)](http://nodejs.org/api/modules.html#modules_module_require_id) (_not supported on Android_)
* [module.id](http://nodejs.org/api/modules.html#modules_module_id)
* [module.filename](http://nodejs.org/api/modules.html#modules_module_filename)
* [module.loaded](http://nodejs.org/api/modules.html#modules_module_loaded)
* [module.parent](http://nodejs.org/api/modules.html#modules_module_parent) (_not yet implemented_)
* [module.children](http://nodejs.org/api/modules.html#modules_module_children) (_not yet implemented_)

### Use the Titanium require()

Just in case you still need to use the old `require()` from Titanium, it's still accessible via `tirequire()`.

```js
require('/foo') === tirequire('foo')
```

## FAQ

* [Why is this so cool?](#why-is-this-so-cool)
* [Should I use tiger8-commonjs.js?](#should-i-use-tiger8-commonjsjs)
* [How does it work?](#how-does-it-work)
* Why is this solution so complicated?
	* [Why can't I just create a new `require` variable?](#why-cant-i-just-create-a-new-require-variable)
	* [Why can't I just override `require()` in my app.js?](#why-cant-i-just-override-require-in-my-appjs)
* [What are the caveats?](#what-are-the-caveats)

### Why is this so cool?

Because now you can start leveraging the power of node.js and npm package management in your Alloy apps.

```bash
$ npm install --prefix ./app/lib ti-mocha should underscore
```

#### alloy.js
```js
var should = require('should/should'), // require the broswer-compatible version in should
	_ = require('underscore');
require('ti-mocha');

describe('ti-commonjs', function() {
	it('should work', function() {
		_.each([1,2,3], function(num) {
			num.should.equal(num);
		});
	});
});
mocha.run();
```

### Should I use `ti-commonjs.js`?

#### cons

* It will probably break your existing Titanium code. The primary reason for this is fundamental difference in specifying absolute paths with Titanium's `require()` vs. `ti-commonjs.js`. This example illustrates, assuming that a module exists at `Resources/foo/bar.js`:
```js
// This is how it's done with Titanium's require()
require('foo/bar');

// and this is how its done with ti-commonjs.js
require('/foo/bar');
```

#### pros

* This is the CommonJS implementation and `require()` usage that will be supported in Titanium 4.0 (Lovingly being referred to as [Ti.Next](http://www.appcelerator.com/blog/2013/09/updates-on-ti-next/)). You can start future-proofing your apps now.
* You get all of the great features listed in the [Usage](#usage) section.
* You can install and distribute modules via [npm](https://www.npmjs.org/)! no more digging through github or Q&A posts.
* Eliminates all platform-specific disparities in Titanium's CommonJS implementation.
* It becomes _much_ easier to port existing node.js modules to Titanium. Many you'll be able to use now without any modifications.
* It is much easier for incoming node.js developers to start using Titanium with this more familiar CommonJS implementation.

### How does it work?

`ti-commonjs.js` overides the existing Titanium `require()` to have node.js-style functionality. It sits directly on top of Titanium's existing module implementation so all module caching is preserved, no wheels are re-invented. It does this by invoking the main `ti-commonjs` function with the current `__dirname` then returns a curried function as the new `require()`.

To truly make the usage seamless, though, your generated Javascript files need a CommonJS wrapper, much like is done in the underlying engine itself. The wrapper looks like this:

**app.js**
```js
(function(_require,__dirname,__filename) {
	var require = _require('ti-commonjs')(__dirname);

	// your code..

})(require,'/','/app.js');
```

### Why can't I just create a new `require` variable?

Because you'd be conflicting with the `require` already in the scope of every module.

```js
var require = require('ti-commonjs'); // CONFLICT with global require
```

### Why can't I just override `require()` in my app.js?

I should just be able to take advantage of Titanium's scoping with respect to the app.js file and have `require()` overridden everywhere, right? Well, you're right, but that's where the problem lies. The issue is that `require()` needs to be executed relative to the current file's directory when it comes to relative paths. This is further compounded by properties like `require.paths`. Globally overriding `require()`, though, will make all paths relative to `Resources`. Let me demonstrate.

**app.js**
```js
require = require('ti-commonjs');
require('/1/2/3/threeModule')();
```

**1/2/3/threeModule.js**
```js
module.exports = function() {
	// DISASTER! You'd think you were referencing '/1/2/3/../twoModule' here,
	// but because the relative directory was established in the app.js
	// you are instead referencing '/../twoModule'. This will end in a
	// runtime error.
	require('../twoModule')();
};
```

**1/2/twoModule.js**
```js
module.exports = function() {
	console.log();
};
```

### What are the caveats?

* `module.parent` and `module.children` cannot be supported since the underlying Titanium `require()` provides no means to get them or assign them to a module. To be able to support this, a change would be required in Titanium. Fortunately, these are rarely used.
* `module.require` is not supported on Android. Details [here](https://github.com/tonylukasavage/ti-commonjs/issues/19).
* This implementation does not load modules with the `.node` extension, as those are for node.js compiled addon modules, which make no sense in the context of Titanium.
* `ti-commonjs.js` does not load from global folders (i.e., `$HOME/.node_modules`), as they are not relevant to mobile app distributions.