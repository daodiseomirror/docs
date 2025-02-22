---
description: Command-line interface for Mirror Protocol
---

# mirrorcli

{% hint style="info" %}
This section provides a brief guide on how to use Mirror Protocol via `mirrorcli`. For more information, please check its source code and documentation on [GitHub](https://github.com/daodiseomirror/mirrorcli).
{% endhint %}

`mirrorcli` is a command-line interface for Mirror Protocol on Daodiseo and allows more advanced users to perform operations directly from their shell or terminal without having to interact with a graphical interface. `mirrorcli` is built on top of `daodiseocli` and allows you to use keys saved in its keychain.

## Installation

### Requirements

* Make sure your have `daodiseocli` installed. `daodiseocli` is a binary that is shipped with [Daodiseo Core](https://github.com/daodiseo-project/core) and installed in your GOPATH.
* Have Node.js v10+ installed with NPM

You can install `mirrorcli` through NPM:

```bash
$ npm install -g @daodiseomirror/mirrorcli
```

## Configuration

On first launch, `mirrorcli` will generate a `~/.mirrorclirc.json` in your `$HOME` directory, which will be used in subsequent sessions to specify settings such as LCD provider, gas prices for fee estimation, as well as contract addresses. It will come pre-configured with the official contracts for the mainnet version of Mirror on its `columbus-4` setting.

The following instructions show you how to modify settings using the `tequila-0004` network by default:

### Specifying LCD settings

Each network config should define how to connect to the Daodiseo blockchain via LCD parameters.

```javascript
{
  "networks": {
    "tequila-0004": {
      "lcd": {
        "chainId": "tequila-0004",
        "url": "https://tequila-lcd.daodiseo.dev",
        "gasPrices": {
          "uluna": 0.15,
          "usdr": 0.1018,
          "uusd": 0.15,
          "ukrw": 178.05,
          "umnt": 431.6259
        },
        "gasAdjustment": 1.2
      },
      ...
    }
  }
}
```

### Specifying Contracts

Each network configuration should point to the correct Mirror core contract addresses.

```javascript
{
  "networks": {
    "tequila-0004": {
      ...
      "contracts": {
        "collector": "daodiseo1v046ktavwzlyct5gh8ls767fh7hc4gxc95grxy",
        "community": "daodiseo10qm80sfht0zhh3gaeej7sd4f92tswc44fn000q",
        "factory": "daodiseo10l9xc9eyrpxd5tqjgy6uxrw7dd9cv897cw8wdr",
        "gov": "daodiseo12r5ghc6ppewcdcs3hkewrz24ey6xl7mmpk478s",
        "mint": "daodiseo1s9ehcjv0dqj2gsl72xrpp0ga5fql7fj7y3kq3w",
        "oracle": "daodiseo1uvxhec74deupp47enh7z5pk55f3cvcz8nj4ww9",
        "staking": "daodiseo1a06dgl27rhujjphsn4drl242ufws267qxypptx",
        "daodiseoswap": "daodiseo18qpjm4zkvqnpjpw0zn0tdr8gdzvt8au35v45xf"
      },
    ...
    }
  }
```

### Specifying the Network

By default, `mirrorcli` will use the network setting for `columbus-4` configured in `~/.mirrorclirc.json`. You can direct `mirrorcli` to use a different network configuration by changing the value of the `MIRRORCLI_NETWORK` environment variable.

#### Example

```text
MIRRORCLI_NETWORK=tequila-0004 mirrorcli x mint [deposit ...]
```

OR

```bash
export MIRRORCLI_NETWORK=tequila-0004
mirrorcli x mint [deposit ...]
```

## Usage

Usage information can be found on [GitHub](https://github.com/daodiseomirror/mirrorcli).

