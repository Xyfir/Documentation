# Manifest File Format

A xyButtons manifest file can be used in Github repositories (not Gists) to further control your button's script and its metadata.

The manifest file *must* be named `xybuttons.manifest.json` and *must* reside within the repository's root directory.

## Properties

All properties other than `scriptFiles` are optional. If you have no need for any of the file's properties then you can most likely avoid including a manifest file.

- `manifestVersion`: `number` - Used by xyButtons to know what properties and values to expect. Currently this should only ever be set to `2`.
- `version`: `string` - The version of your button. This property currently has no affect, but may be required in the future once xyButtons has a proper button version system.
- `scriptFiles`: `string[]` - An array of paths for files that will be included in your button's script. Example values: `['my-file.js', 'path/to/file.js']`. Each file must be explicitly listed, you cannot as of yet simply include an entire directory.
- `button`: `object` - An object containing metadata for your button that will be used to override the data provided in the xyButtons client when updating or creating the button. All properties within the object are optional.
  - `name`: `string` - The button's name.
  - `urlMatch`: `string` - A regular expression string that the client's current url must match if button is to be inserted. Writing your URL match expression within JSON can get confusing because you need to escape your backslashes.
  - `description`: `string` - The button's description. Markdown is supported but should be avoided in the description's first line.
  - `domains`: `string` - A comma delimited list of domains that the button should work on. This is used strictly for searching and has no impact on the button's behavior. Can be `'*'` to denote a global matching button or `'**'` to denote a button that matches too many sites to list.
  - `tooltip`: `string` - The text displayed in a tooltip when the button is hovered over.
  - `styles`: `object` - An `HTMLElement.style` object containing the button's custom styles.
  - `content`: `string` - The button's text content. A URI encoded string that can contain any characters, including emojis.