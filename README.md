# RawCull Documentation

Source for the [RawCull website](https://techrawcull.netlify.app/), built with Hugo and the Docsy theme.

## Content Layout

- `content/en/_index.md`: homepage and entry points.
- `content/en/docs`: RawCull quick start, task guides, settings, privacy, and the two screenshot sections.
- `content/en/rawcullbrowse`: the companion browser's overview and feature guide.
- `content/en/blog/releases`: dated release history, including source-development records.
- `content/en/about`: developer background and contact information.
- `static/images`: screenshots referenced by the content.
- `hugo.yaml`: navigation, site metadata, theme, and permalink settings.

The documentation sidebar follows the review workflow: culling, sharpness and focus checks, similarity, local AI, screenshots, then settings and maintenance. Keep practical instructions separate from model reference material and historical release details.

## Local Preview and Build

Use the Hugo Extended version declared in `package.json` and `hugo.yaml`, with the project's Node and Sass dependencies available.

```sh
npm run serve
npm run build:production
```

The production build writes to `public`. Netlify uses the configuration in `netlify.toml` when published changes reach `main`.

Release-post URLs include their publication date. Use Hugo `relref` rather than guessing a sibling URL. When moving a published documentation page, retain its old URL in front matter `aliases`.

See [CONTRIBUTING.md](CONTRIBUTING.md) for documentation changes and issue reports.
