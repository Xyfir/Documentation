While it's possible to create and manage button scripts directly in the xyButtons extension, you'd probably rather work on your button scripts in your preferred coding environment as well as take advantage of Git for a version control system. Xyfir Buttons' support for remote Github repositories (and Gists) allows you to do this.

There are however some important things you should be aware of before you begin putting your buttons in remote repos.

# Github Repositories

- A repository *must* contain a non-empty `main.js` file in the root of the repository.
- A repository *may* contain a `xybuttons.manifest.json` file which will allow you to have more control over your repository and the button it is linked to.
- If the repository *does not* contain a `xybuttons.manifest.json` file, all files within the root folder will be added to the button's script.
- The repository link provided to xyButtons *must* be in the following format: `https://github.com/:user/:repoId`.

# Github Gists

Due to limitations applied to Github's Gists, you should probably only utilize this feature for simple (usually single-file) scripts.

- Gists cannot contain folders, so all files will be within the root of the repository.
- A Gist *must* contain a non-empty `main.js` file.
- Unlike normal repositories, if a Gist contains a `xybuttons.manifest.json` file it will be ignored.
- All files in the Gist will be added to the button's script.
- The Gist link provided to xyButtons *must* be in the following format: `https://gist.github.com/:user/:gistId`.