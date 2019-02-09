# Buttons

A button is a single, self-contained button element that is inserted into any web page where both its url match string matches the current page's url, and the preset's url match string in which the button resides also matches the current url.

Buttons can use JavaScript to interact with and manipulate the page in which it is inserted. Just about anything an actual user can do and access, a button can do as well, programmatically via JavaScript. A button can be used to run JavaScript code either when the button is inserted into a page, or upon an event, like the button being clicked.

Every button can have its own unique style, based on the CSS provided by both the button creator and the preset creator.

# Presets

A preset is a container that brings multiple buttons together into a single entity. Only one preset can be inserted into a page at a time.

A preset also handles the placement and sizing of its buttons on the page. The preset creator sets the default placement and sizing for the buttons, but users of the preset can override that placement if they wish. A preset can also override a button's styling to better fit the style of the other buttons in the preset, and the overall theme of the preset.

## System Preset

The system preset contains buttons created by Xyfir Buttons, that relate directly either to the Xyfir Buttons extension itself, or to the active user preset. The system preset can be accessed by clicking the button titled 'xy', that is added to every preset. When that button is clicked, the active preset is hidden (but not removed), and the system preset's buttons are inserted into the page.

# URL Match Strings

A url match string is a [regular expression](https://regexone.com/) that tells the Xyfir Buttons extension if a preset or button should be injected into a page. Anytime a preset's url match string matches a page's url (the entire thing, protocol, host, hash, etc), that preset can be injected into the page. If multiple presets match a page, only one is injected, and the rest can be accessed by changing the active preset via Xyfir Buttons' system preset.

Just because a preset's url match string matches a page's url, does not mean its buttons will be injected. The buttons must also match the page's url before they can be inserted.

A URL match string that matches all possible urls is called a 'global' match.

# Keyboard Commands

`SHIFT + ALT + X` enters Xyfir Buttons into listening mode. The next key pressed (after letting go of the previous keys), will trigger an action if the key is matched to a command.

## Hide Preset

Pressing the `s` key after entering listen mode will toggle the visibility of the active preset. Use this to hide a preset's buttons while still allowing the scripts of those buttons to run.