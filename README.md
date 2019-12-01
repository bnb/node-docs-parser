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
