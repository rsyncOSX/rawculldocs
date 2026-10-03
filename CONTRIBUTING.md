# Contributing to the RawCull Documentation

Report a documentation problem or propose a correction through the [repository issue tracker](https://github.com/rsyncOSX/rawculldocs/issues). Include the page URL, the app version where relevant, and the wording or behavior that needs correction.

For a text change, edit the Markdown in `content/en` and open a pull request. Describe what changed and why. Keep instructions focused on the user's task, use the control labels shown in the app, and distinguish released behavior from source-development notes.

Place screenshots in `static/images` and provide accurate alt text. Check image paths and internal links when renaming a page; preserve an alias for an existing published URL. Use Hugo `relref` links for dated release posts so the configured permalink is resolved correctly.

Compile and inspect the affected pages before publishing. See [README.md](README.md) for the repository layout and build commands.
