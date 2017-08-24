# Button

A button's script starts with a `main.js` file that is run when the button is injected into the page. This file has access to a variable called `Button` that contains some very simple methods and properties relating to the button. Since a button is injected into each page where it and its parent preset match the current URL, the script has complete access to the DOM and any global variables saved to the page.

## Properties

```js
/**
 * @type {object} The button's data as returned from the xyButtons API.
 */
Button.data;

/**
 * @type {HTMLDivElement} The `.xybuttons` container element in which the
 * button's HTMLButtonElement resides.
 */
Button.container;

/**
 * @type {HTMLButtonElement} The button's actual button element within the
 * `.xybuttons` container element.
 */
Button.element;
```

## Methods

```js
/**
 * A shorcut for `Button.element.addEventListener(...)`.
 * https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener
 */
Button.on();

/* Examples */

Button.on('click', event => {
  // Prints 'hello' to console when Button.element is clicked
  console.log('hello');
});

Button.on('contextmenu', event => {
  // Prints 'world' to console when 'contextmenu' event is fired on button element
  console.log('world');
});
```

```js
/**
  * Requires a local script file or a remote file. The page's CSP is (mostly)
  * bypassed.
  * @param {string} resource - Can either be the path and file name of a local
  * file within the script object OR a remote file to be downloaded via
  * Button.request().
  * @param {boolean} [asModule] - Only has an affect when the resource being
  * required is a remote JavaScript file. If true, `exports` and
  * `module.exports` objects are made available for the loaded script to use.
  * Whichever variable was modified is then returned via the promise's resolve
  * with a default of an empty object if the script doesn't use the variables.
  * @returns {Promise|any} Returns a promise if a remote file is being loaded.
  * Otherwise whatever is exported in the local file is returned.
  */
Button.require(resource, asModule);

/* Examples */
(async function(){
  // Load a local file that is part of the button's script
  const someFunction = Button.require('path/to/local-file.js');
  const someObject = Button.require('config.js');
  
  // jQuery is made global
  await Button.require('https://code.jquery.com/jquery-X.X.X.min.js');

  // jQuery is now only accessible via this `$` variable.
  const $ = await Button.require(
    'https://code.jquery.com/jquery-X.X.X.min.js',
    true // Load file as module
  );

  // CSS is inserted into the page in a <style> element if page's CSP allows
  // The promise resolves to the element on the element's `onload` event
  const style = await Button.require('https://site.com/some-css-file.css');

  // Files that aren't CSS or JS will have their request response returned
  // for the user to handle
  const res = await Button.require('https://site.com/some-file.txt');
})();
```

```js
/**
  * An imitation of the superagent request library's API with a few changes.
  * The main difference is that .end() does not accept a callback function and
  * instead returns a promise.
  * See https://github.com/visionmedia/superagent/blob/master/docs/index.md
  * Requests sent through Button.request() bypass the current page's CSP.
  */
Button.request;

/* Examples */

Button.request
  .post('https://somesite.com/some/route')
  .query({ queryStringVariable: 1 })
  .send({ formValue: 'test' })
  .end()
  .then(res => {
    // res == superagent's response object
  })
  .catch(err => {
    // err == either superagent's error or an error emitted somewhere during
    // the transfer process
  })
```

## Storage Methods

Each button also has access to its own storage, which will persist across all domains. Data is stored within a single object.

```js
/**
 * Returns the button's storage object.
 * @returns {Promise} A promise that resolves to the storage object and
 * rejects with an error message.
 */
Button.storage.get()
  .then(storage => {
    // storage is your entire storage object
  })
  .catch(err => {
    // could not load storage
  });
```

```js
/**
 * Sets a value to a key in the button's storage object. The object
 * passed is merged with the existing object. This means if a key already
 * exists, it will be replaced, and if a key is not present in the new object
 * that exists in the old one, it will remain untouched.
 * @param {object} obj - A key:value object to merge the button's storage
 * object with via Object.assign().
 * @returns {object} The updated storage object.
 */
Button.storage.set(obj);

/* Examples */

let storage = {};

storage = Button.storage.set({ test: 'val' });
// storage is now `{ test: 'val' }`

storage = Button.storage.set({ another: 500 });
// storage is now `{ test: 'val', another: 500 }`

storage = Button.storage.set({ test: { key: 'val' }, hello: 'world' });
// storage is now `{ test: { key: 'val' }, another: 500, hello: 'world' }`
```

```js
/**
 * Removes keys from the button's storage.
 * @param {string[]} keys - The keys to remove from the button's storage.
 * @returns {object} The updated storage object.
 */
Button.storage.remove(keys);

/* Examples */

storage = Button.storage.remove(['test']);
// storage is now `{ another: 500, hello: 'world' }`

storage = Button.storage.remove(['another', 'hello']);
// storage is now `{ }`
```

```js
/**
 * Replaces the entire storage object with a new object.
 * @param {object} obj - The object to replace the current object with.
 */
Button.storage.replace(obj);

/* Examples */

storage = Button.storage.replace({ some: 'value' });
// storage is now `{ some: 'value' }`

storage = Button.storage.replace({ test: 1, hello: 'world' });
// storage is now `{ test: 1, hello: 'world' }`
```

```js
/**
 * Empties the storage object. Does not return anything.
 */
Button.storage.clear();
```

# Script Files

## main.js

Every button script *must* have a `main.js` file as this is the file that is ran immediately upon being injected into a page. If you want your button's script to only run when it is clicked or on some other event, you must wrap that code within an event listener via `Button.on()` or one of the browser's native event listener APIs.

## Other Files

The contents of files other than `main.js` are not ran until they are loaded via `Button.require()`. Files other than `main.js` are exposed to two variables that they can use to export code: `exports` and `module.exports`. You can add properties to or override entirely either `exports` or `module.exports` and whichever variable was modified will be returned via `Button.require()` with `exports` taking precedence over `module.exports`.

```js
// main.js
const func = Button.require('function.js'); // function(...) {...}
const obj = Button.require('object.js'); // { num, str, func, obj, test }
const num = Button.require('number.js'); // 10
Button.require('folder/no-export.js'); // prints 'hello' to console
```

```js
// function.js
exports = function(arg1, arg2) { return arg1 + arg2; }
```

```js
// object.js
module.exports = {
  num: 1, str: 'hello', func: () => 1, obj: {}
}
module.exports.test = 'a' + 'b';
```

```js
// number.js
// Both variables were changed, but exports takes precedence
module.exports = 5;
exports = 10;
```

```js
// folder/no-export.js
// Does not export anything, but its contents are ran once required
const str = 'hello';
console.log(str);
```

While other files can be placed in folders, there are no relative paths. Using `./` or `../` in your path string will cause an error.

# Important Notes

The button is injected into the page as soon as possible, which means that sometimes the button is rendered *before* the page's `body` element has completed rendering or before some resources have finished downloading. Your scripts should not assume that elements and global variables that it relies on will be there and you should add code to check if they exist before running.

You should keep in mind the [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) of the pages that your script is supposed to run on. A CSP can prevent you from doing things that you might not expect. Xyfir Buttons modifies, but does not remove, a site's CSP to allow the use of 'unsafe eval', which mainly allows you to use JavaScript's `eval()` in your code.