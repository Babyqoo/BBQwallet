# BaboQoo Wallet

[![Github All Releases](https://img.shields.io/github/downloads/ethereum/mist/total.svg)]()
[![Build Status develop branch](https://travis-ci.org/ethereum/mist.svg?branch=develop)](https://travis-ci.org/ethereum/mist)
[![Join the chat at https://gitter.im/ethereum/mist](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/ethereum/mist)
[![Code Triagers Badge](https://www.codetriage.com/ethereum/mist/badges/users.svg)](https://www.codetriage.com/ethereum/mist)


The Mist browser is the tool of choice to browse and use Ðapps.

For the Mist API see the [MISTAPI.md](MISTAPI.md).

Please note that this repository is the Electron host for the Meteor based wallet dapp whose repository is located here: https://github.com/ethereum/meteor-dapp-wallet.

## Help and troubleshooting

In order to get help regarding Mist or BaboQoo Wallet, please follow:

1. Please check the [Mist troubleshooting guide](https://github.com/Babyqoo/BBQwallet/wiki).
1. Go to the [Gitter Channel](https://gitter.im/ethereum/mist) to connect with the community for instant help.
1. Search for [similar issues](https://github.com/Babyqoo/BBQwallet/issues?q=is%3Aopen+is%3Aissue+label%3A%22Type%3A+Canonical%22) and potential help.
1. Or create a [new issue](https://github.com/Babyqoo/BBQwallet/issues).

## How to contribute

Contributions via Pull Requests are so welcome. You can see where to help looking for issues with the [Enhancement](https://github.com/Babyqoo/BBQwallet/issues?q=is%3Aopen+is%3Aissue+label%3A%22Type%3A+Enhancement%22) or [Bug](https://github.com/Babyqoo/BBQwallet/issues?q=is%3Aopen+is%3Aissue+label%3A%22Type%3A+Bug%22) labels. We can help guiding you towards the solution.

You can also help by [responding to issues](https://github.com/Babyqoo/BBQwallet/issues?q=is%3Aissue+is%3Aopen+label%3A%22Status%3A+Triage%22). Sign up on [CodeTriage](https://www.codetriage.com/ethereum/mist) and it'll send you gentle notifications with a configurable frequency. It is a nice way to help while learning.

## Installation

If you want to install the app from a pre-built version on the [release page](https://github.com/Babyqoo/BBQwallet/releases),
you can simply run the executeable after download.

For updating simply download the new version and copy it over the old one (keep a backup of the old one if you want to be sure).

### Linux .zip installs

In order to install from .zip files, please install `libgconf2-4` first:

```bash
apt-get install libgconf2-4
```

### Config folder
The data folder for Mist is stored in other places:

- Windows `%APPDATA%\Baboqoo`
- macOS `~/Library/Application\ Support/Baboqoo`
- Linux `~/.config/Baboqoo`


## Development

For development, a Meteor server will need to be started to assist with live reload and CSS injection.
Once a Mist version is released the Meteor frontend part is bundled using the `meteor-build-client` npm package to create pure static files.

### Dependencies

To run mist in development you need:

- [Node.js](https://nodejs.org) `v7.x` (use the prefered installation method for your OS)
- [Meteor](https://www.meteor.com/install) javascript app framework
- [Yarn](https://yarnpkg.com/) package manager
- [Electron](http://electron.atom.io/) `v1.7.11` cross platform desktop app framework
- [Gulp](http://gulpjs.com/) build and automation system

Install the latter ones via:

```bash
$ curl https://install.meteor.com/ | sh
$ curl -o- -L https://yarnpkg.com/install.sh | bash
$ yarn global add electron@1.7.11
$ yarn global add gulp
```

### Initialisation

Now you're ready to initialise BaboQooWallet for development:

```bash
$ git clone https://github.com/Babyqoo/BBQwallet.git
$ cd bbqwallet
$ yarn
```

To update BaboQooWallet in the future, run:

```bash
$ cd bbqwallet
$ git pull
$ yarn
```

### Run BaboQooWallet

For development we start the interface with a Meteor server for autoreload etc.
*Start the interface in a separate terminal window:*

```bash
$ cd bbqwallet/interface && meteor --no-release-check
```

In the original window you can then start BaboQooWallet with:

```bash
$ cd bbqwallet
$ yarn dev:electron
```

*NOTE: client-binaries (e.g. [geth](https://github.com/babyqoo/baboqoo)) specified in [clientBinaries.json](https://github.com/Babyqoo/BBQwallet/blob/master/clientBinaries.json) will be checked during every startup and downloaded if out-of-date, binaries are stored in the [config folder](#config-folder)*

*NOTE: use `--help` to display available options, e.g. `--loglevel debug` (or `trace`) for verbose output*

### Run the Wallet

Start the wallet app for development, *in a separate terminal window:*

```bash
$ cd bbqwallet/interface && meteor --no-release-check
```

In another terminal:

```bash
$ cd my/path/meteor-dapp-wallet/app && meteor --port 3050
```

In the original window you can then start Mist using wallet mode:

```bash
$ cd bbqwallet
$ yarn dev:electron --mode wallet
```

### Connecting to node via HTTP instead of IPC

This is useful if you have a node running on another machine, though note that
it's less secure than using the default IPC method.

```bash
$ yarn dev:electron --rpc http://localhost:8545
```


### Passing options to Geth

You can pass command-line options directly to Geth by prefixing them with `--node-` in
the command-line invocation:

```bash
$ yarn dev:electron --mode mist --node-rpcport 19343 --node-networkid 2
```

The `--rpc` BaboQooWallet option is a special case. If you set this to an IPC socket file
path then the `--ipcpath` option automatically gets set, i.e.:

```bash
$ yarn dev:electron --rpc /my/geth.ipc
```

...is the same as doing...


```bash
$ yarn dev:electron --rpc /my/geth.ipc --node-ipcpath /my/geth.ipc
```

### Creating a local private net

See this guide to quickly set up a local private network on your computer:
https://gist.github.com/evertonfraga/9d65a9f3ea399ac138b3e40641accf23


### Using BaboQooWallet with a privatenet

To run a private network you will need to set the IPC path, network id and data
folder:

```bash
$ yarn dev:electron --rpc ~/Library/BaboQoo/geth.ipc --node-networkid 1234 --node-datadir ~/Library/BaboQoo/privatenet
```

_NOTE: since `ipcpath` is also a Mist option you do not need to also include a
`--node-ipcpath` option._

You can also launch `geth` separately with the same options prior starting
BaboQooWallet.


### Deployment

Our build system relies on [gulp](http://gulpjs.com/) and [electron-builder](https://github.com/electron-userland/electron-builder/).

#### Dependencies

[meteor-build-client](https://github.com/frozeman/meteor-build-client) bundles the [meteor](https://www.meteor.com/)-based interface. Install it via:

```bash
$ npm install -g meteor-build-client
```

Furthermore cross-platform builds require additional [`electron-builder` dependencies](https://github.com/electron-userland/electron-builder/wiki/Multi-Platform-Build#linux). On macOS those are:


Windows deps:
```bash
$ brew install wine --without-x11 mono makensis
```

Linux deps:
```bash
$ brew install gnu-tar libicns graphicsmagick xz
```

#### Generate packages

To generate the binaries for Mist run:

```bash
$ gulp
```

To generate the BaboQoo Wallet (this will pack the one Ðapp from https://github.com/ethereum/meteor-dapp-wallet):

```bash
$ gulp --wallet
```

The generated binaries will be under `dist_baboqoowallet/release` or `dist_wallet/release`.

#### Options

##### platform

To build binaries for specific platforms (default: all available) use the following flags:

```bash
$ gulp --mac      # mac
$ gulp --linux    # linux
$ gulp --win      # windows
```

##### walletSource

With the `walletSource` you can specify the Wallet branch to use, default is `master`:

    $ gulp --wallet --walletSource develop

Options are:

- `master`
- `develop`
- `local` Will try to build the wallet from [mist/]../meteor-dapp-wallet/app

*Note: applicable only when combined with `--wallet`*

##### skipTasks

When building a binary, you can optionally skip some tasks — generally for testing purposes.

```bash
$ gulp --mac --skipTasks=bundling-interface,release-dist
```

##### Checksums

Spits out the MD5 checksums of distributables.

It expects installer/zip files to be in the generated folders e.g. `dist_mist/release`

```bash
$ gulp checksums [--wallet]
```

#### Cutting a release

1. Install [release](https://github.com/zeit/release) globally:

    ```bash
    $ yarn global add release
    ```

2. Create a git tag and a GitHub release:

    ```bash
    $ release <major|minor|patch>
    ```

3. A generated release draft will open in the default browser. Edit the information and add assets as necessary.

## Testing

Tests are ran using [Spectron](https://github.com/electron/spectron/), a webdriver.io runner built for Electron.

First make sure to build BaboQooWallet with:

```bash
$ gulp
```

Then run the tests:

```bash
$ gulp test
```

*Note: Integration tests are not yet supported on Windows.*
