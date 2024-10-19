<div align="center">

# npm-deprecate

**Programmatically deprecate your NPM published packages**

[![GitHub](https://img.shields.io/github/license/favware/npm-deprecate)](https://github.com/favware/npm-deprecate/blob/main/LICENSE)
[![npm](https://img.shields.io/npm/v/@favware/npm-deprecate?color=crimson&logo=npm)](https://www.npmjs.com/package/@favware/npm-deprecate)

[![Support Server](https://discord.com/api/guilds/512303595966824458/embed.png?style=banner2)](https://join.favware.tech)

</div>

## Description

When working on larger libraries it may be desirable to release every commit to
NPM automatically using a GitHub workflow. However when doing this you'll end up
with a LOT of versions on npm, which gets extremely cluttered.

To solve this, one can use this package to programmatically deprecate many
versions at once, matching a glob that is checked against the version name.

## Installation

You can use the following command to install this package, or replace
`npm install -D` with your package manager of choice.

```sh
npm install -D @favware/npm-deprecate
```

Or install it globally:

```sh
npm install -g @favware/npm-deprecate
```

Then call the script with `npm-deprecate` or `nd`:

```sh
npm-deprecate --name "*next*" --package "@favware/npm-deprecate" # Add any other flags or use --help
nd --name "*next*" --package "@favware/npm-deprecate" # Add any other flags or use --help
```

Alternatively you can call the CLI directly with `npx`:

```sh
npx @favware/npm-deprecate --name "*next*" --package "@favware/npm-deprecate" # Add any other flags or use --help
```

## Usage

### Environment Variables

The following environment variables have to be set before running this script:

| Name              | Required | Description                                                     |
| ----------------- | -------- | --------------------------------------------------------------- |
| `NODE_AUTH_TOKEN` | Yes      | The NPM Automation Token that can be used to deprecate versions |

### Configuration

You can provide all options through CLI flags:

```sh
Usage: npm-deprecate [options]

Options:
  -V, --version                                output the version number
  -n, --name <nameGlob>                        A glob pattern that will determine which packages are deprecated. Anything that passes
                                               [Micromatch](https://www.npmjs.com/package/micromatch) will work here. For example set `*dev*` to match `13.2.0-dev.123a`.
  -d, --deprecate-dist-tag [deprecateDistTag]  Whether the version that is in the current dist tags should be preserved or not. By default dist tags are preserved. When set
                                               to `true`, dist tags are pruned. (default: false)
  -m, --message [message]                      A custom message to show for all the deprecated versions. (default: "This version has been automatically deprecated by
                                               @favware/npm-deprecate. Please use a newer version.")
  -v, --verbose                                Print verbose information (default: false)
  -p, --package <packages...>                  Repeatable, each will be treated as another package. The packages that should be deprecated
  -h, --help                                   display help for command
```

Or, you can set most of these options through a configuration file. This file
should be located at your current working directory (where you're calling this
package). It should be named `.npm-deprecaterc`, optionally suffixed with
`.json`, `.yaml`, or `.yml`.

### Config file fields

- `--name` maps to `name`
- `--deprecate-dist-tag` maps to `deprecateDistTag`
- `--verbose` maps to `verbose`
- `--message` maps to `message`
- `--package` maps to `package`

When using `.npm-deprecaterc` or `.npm-deprecaterc.json` as your config file you
can also use the JSON schema to get schema validation. To do so, add the
following to your config file:

```json
{
  "$schema": "https://raw.githubusercontent.com/favware/npm-deprecate/main/assets/npm-deprecate.schema.json"
}
```

**Example JSON file**:

```json
{
  "$schema": "https://raw.githubusercontent.com/favware/npm-deprecate/main/assets/npm-deprecate.schema.json",
  "name": "*next*",
  "deprecateDistTag": false,
  "verbose": true,
  "package": ["@favware/cliff-jumper", "@favware/npm-deprecate"]
}
```

**Example YAML file**:

```yaml
name: '*next*'
deprecateDistTag: false
verbose: true
package:
  - '@favware/cliff-jumper'
  - '@favware/npm-deprecate'
```

### Default values

This library has opinionated defaults for its options. These are as follows:

- `--deprecate-dist-tag` will default to `false`.
- `--message` will default to
  `This version has been automatically deprecated by @favware/npm-deprecate. Please use a newer version.`.
- `--verbose` will default to `false`.

### Using this in a GitHub Workflow

```yaml
name: NPM Auto Deprecate

on:
  schedule:
    - cron: '0 0 * * *'

jobs:
  auto-deprecate:
    name: NPM Auto Deprecate
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Project
        uses: actions/checkout@v2
      - name: Use Node.js v18
        uses: actions/setup-node@v2
        with:
          node-version: 18
          cache: yarn
          registry-url: https://registry.npmjs.org/
      - name: Install Dependencies if Cache Miss
        run: yarn --immutable
      - name: Deprecate versions
        run: yarn npm-deprecate
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_PUBLISH_TOKEN }}
```

## Buy us some doughnuts

Favware projects are and always will be open source, even if we don't get
donations. That being said, we know there are amazing people who may still want
to donate just to show their appreciation. Thank you very much in advance!

We accept donations through Ko-fi, Paypal, Patreon, GitHub Sponsorships, and
various cryptocurrencies. You can use the buttons below to donate through your
method of choice.

|   Donate With   |                      Address                      |
| :-------------: | :-----------------------------------------------: |
|      Ko-fi      |  [Click Here](https://donate.favware.tech/kofi)   |
|     Patreon     | [Click Here](https://donate.favware.tech/patreon) |
|     PayPal      | [Click Here](https://donate.favware.tech/paypal)  |
| GitHub Sponsors |  [Click Here](https://github.com/sponsors/Favna)  |
|     Bitcoin     |       `1E643TNif2MTh75rugepmXuq35Tck4TnE5`        |
|    Ethereum     |   `0xF653F666903cd8739030D2721bF01095896F5D6E`    |
|    LiteCoin     |       `LZHvBkaJqKJRa8N7Dyu41Jd1PDBAofCik6`        |

## Contributors

Please make sure to read the [Contributing Guide][contributing] before making a
pull request.

Thank you to all the people who already contributed!

<a href="https://github.com/favware/npm-deprecate/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=favware/npm-deprecate" />
</a>

[contributing]: .github/CONTRIBUTING.md
