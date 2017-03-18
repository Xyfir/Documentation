# Button

A button's script starts with a `main.js` file that is run when the button is injected into the page. This file has access to a variable called `Button` that contains some very simple methods and properties relating to the button. Since a button is injected into each page where it and its parent preset match the current URL, the script has complete access to the DOM and any global variables saved to the page.

```js
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

/**
 * A simple method for applying event listeners to Button.element. Only one
 * event of the same type can exist at one time with this method. Events
 * added to Button.element here will automatically be removed when the button
 * instance is destroyed.
 * @param {string} eventName - The event to listen for, like 'click'.
 * @param {function} listener - A function that is called when an event of
 * eventName is emitted.
 */
Button.on(eventName, listener);

/* Examples */

Button.on('click', event => {
  // Prints 'hello' to console when Button.element is clicked
  console.log('hello');
});

/**
 * Runs a script file and returns its result. Other files will NOT have access
 * to the `Button` variable unless passed from the main.js file.
 * @param {string} file - A string containing the path and file name of the
 * file to load.
 */
Button.load(file);

/* Examples */

const func = Button.load('my-file.js')
func('hello');

// prints 'hello' to console
// where my-file.js is
function(val) { console.log(val); }

//

Button.load('some/path/to/file.js');

// prints 'hello' to console
// where some/path/to/file.js is
console.log('hello');
```

## Storage

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

/**
 * Empties the storage object. Does not return anything.
 */
Button.storage.clear();
```

# Important Notes

The contents of your `main.js` script file are ran immediately upon injection into the page. If you want your button's script to only run when it is clicked or on some other event, you must wrap that code within an event listener via `Button.on()` or one of the browser's native event listener APIs.

The contents of files other than `main.js` are not ran until they are loaded via `Button.load()`. If the loaded file is a function, that code is not ran until the file is loaded and the returned function is called.

The button is injected into the page as soon as possible, which means that sometimes the button is rendered *before* the page's `body` element has completed rendering or before some resources have finished downloading. Your scripts should not assume that elements and global variables that it relies on will be there and you should add code to check if they exist before running.

You should keep in mind the [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) of the pages that your script is supposed to run on. A CSP can prevent you from loading external resources among other things that you might not expect. Xyfir Buttons modifies, but does not remove, a site's CPS to allow the use of 'unsafe eval', which mainly allows you to use JavaScript's `eval()` in your code.