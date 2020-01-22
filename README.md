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
npm run parser # this will build the docs!
```

## Repository Structure

- `/docs/api/`
  - where the API docs live. `electron-docs-parser`looks here by default.
- `/docs/api/structures/`
  - required to exist by `electron-docs-parser`. Not currently in use but maybe probably should be?
- `/originals/`
  - original sources from the Node.js repo for docs in `/docs/api/`
- `/electron-api.json`
  - JSON output. Currently somehwat Electron specfici due to hard-coded-ness of the `@electron/docs-parser` tool, but there's currently a PR open to make this more dynamic ([electron/docs-parser#21](https://github.com/electron/docs-parser/pull/21)).

## Current Blockers

- Several parts of the docs-parser tooling are highly electron-specific [electron/docs-parser#19](https://github.com/electron/docs-parser/issues/19)
- Moving as-is, Node.js would _seemingly_ lose the ability to include meta information (`introduced`, `stability`, and `history`)
  - Having discussed this with Sam from Electron, there are ways we'd be able to address this.
- Currently a bug in which multi-mode outputs incorrect descriptions for additional instances of `Class` in the JSON output. [electron/docs-parser#17](https://github.com/electron/docs-parser/issues/27)

## Lessons Learned

- Converting `querystring` was relatively straightforward. Effectively just converting the default Node.js format to the docs-parser format.
- Converting `v8` was significantly more complex. Certain narrative elements of the Node.js docs – or, at least, the `v8.md` doc - are fundamentally incompatible with the Electron docs and need to either be re-integrated or dropped entirely.
  - This isn't necessarily a positive nor a negative - more of a neutral observation. It could be argued that removing certain kinds of one-off narrative elements could bring better overall consistency to the docs, which could be an overall win.
  - A more specific example is the `Serialization API` section of v8.md in the Node.js docs. This section is a narrative seperation that sets apart an API when it should quite likely be sitting at the same level as the rest of the APIs available from the module. The `Serialization API` heading and commentary was axed, moving the commentary into the actual Class definitions themselves. This was necessary because docs-parser expects certain elements (like `Instance Methods`) to be at certain levels of heading.
- Converting `worker-threads`, I realized how inconsistent several elements of Node.js documentation writing is and how docs-parser forces us into a more consistent folow.
  - Reading the docs prior to docs-parser, it feels like each bit was writen independently of every other bit, leaving me with a disjointed taste. Using docs-parser, everything becomes uniform and much smoother across sections.
  - Despite being even more complex than `v8`, this one actually felt significantly more understandable at the end given the restrictions around `Instance Methods`, `Instance Properties`, and `Instance Events` that lead to less jittery interactions while reading through the documentation and a more consistent and modular narrative structure.
  - Enforces a gramatical distinction between methods that `Return` and properties that are `a`/`an` _something_. While very minor, as a reader this feels very logical and is a helpful distinction.