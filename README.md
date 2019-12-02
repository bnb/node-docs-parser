# node-docs-parser

This is a repo to test the viability of the possibility of suggesting to the Node.js project that we migrate to the [electron/docs-parser](https://github.com/electron/docs-parser) for our documentation rather than continuing with our existing tooling.

## Usage

You will need to clone this repo and install `@electron/docs-parser` globally:

```sh
git clone git@github.com:bnb/node-docs-parser.git
npm i -g @electron/docs-parser
```

next, you'll need to navigate to the direcory and then run the CLI:

```sh
cd node-docs-parser
electron-docs-parser --dir ./ # this will build the docs!
```

## Current Blockers

- Several parts of the docs-parser tooling are highly electron-specific [electron/docs-parser#19](https://github.com/electron/docs-parser/issues/19)
- Moving as-is, Node.js would _seemingly_ lose the ability to include meta information (`introduced`, `stability`, and `history`)

## Outstanding Questions

- For Classes, it is stated that `Constructors must be listed with ###-level titles.` in the Electron Docs Styleguide. However, there doesn't seem to be a direct example in the styleguide. At present, in `v8.md` I'm approaching this by just including the constructor in the text of the Class definition but this should probably be figured out and moved to the correct location.

## Lessons Learned

- Converting `querystring` was relatively straightforward. Effectively just converting the default Node.js format to the docs-parser format.
- Converting `v8` was significantly more complex. Certain narrative elements of the Node.js docs – or, at least, the `v8.md` doc - are fundamentally incompatible with the Electron docs and need to either be re-integrated or dropped entirely.
  - This isn't necessarily a positive nor a negative - more of a neutral observation. It could be argued that removing certain kinds of one-off narrative elements could bring better overall consistency to the docs, which could be an overall win.
  - A more specific example is the `Serialization API` section of v8.md in the Node.js docs. This section is a narrative seperation that sets apart an API when it should quite likely be sitting at the same level as the rest of the APIs available from the module. The `Serialization API` heading and commentary was axed, moving the commentary into the actual Class definitions themselves. This was necessary because docs-parser expects certain elements (like `Instance Methods`) to be at certain levels of heading.
