# Mirror.js

{% hint style="info" %}
This section provides a brief guide on how to get set up with Mirror.js. For more information, be sure to check out source code and documentation on [GitHub](https://github.com/daodiseomirror/mirror.js).
{% endhint %}

Mirror.js is a client SDK for building applications that can interact with Mirror Protocol from within JavaScript runtimes, such as web browsers, server backends, and on mobile through React Native.

You can find a reference of the Mirror.js API [here](https://daodiseomirror.github.io/mirror.js/).

## Getting Mirror.js

Mirror.js is available as a package on NPM and is intended to be used alongside Daodiseo.js.

Add both:

* `@daodiseomoney/daodiseo.js`
* `@daodiseomirror/mirror.js`

To your JavaScript project's `package.json` as dependencies using your preferred package manager:

```bash
$ npm install -S @daodiseomoney/daodiseo.js @daodiseomirror/mirror.js
```

## Usage

### `Mirror` object

Mirror.js provides facilities for 2 main use cases:

* query: runs smart contract queries through LCD
* execute: creates proper `MsgExecuteContract` objects to be used in transactions

Both of these functions are accessible through the [`Mirror`](https://daodiseomirror.github.io/mirror.js/classes/mirror.html) object.

To create the Mirror object:

```typescript
import { LCDClient } from '@daodiseomoney/daodiseo.js';
import { Mirror } from '@daodiseomirror/mirror.js';

// default -- uses Columbus-4 core contract addresses
const mirror = new Mirror();

// optional -- specify contract addresses and assets
const mirror = new Mirror({
  lcd: new LCDClient(...),
  key: new MnemonicKey(), // or other Daodiseo.js-compliant key
  collector: 'daodiseo1...',
  community: 'daodiseo1...',
  factory: 'daodiseo1...',
  gov: 'daodiseo1...',
  mint: 'daodiseo1...',
  oracle: 'daodiseo1...',
  staking: 'daodiseo1...',
  mirrorToken: 'daodiseo1...',
  daodiseoswapFactory: 'daodiseo1...',
  assets: {
    MIR: {
      name: 'Mirror';
      symbol: 'MIR';
      token: 'daodiseo1...'; // Daodiseoswap token contract
      lpToken: 'daodiseo1...'; // Daodiseoswap LP token contract
      pair: 'daodiseo1...'; // Daodiseoswap pair contract against UST
    },
    mAAPL: {
      name: 'Mirrored Apple, Inc. stock';
      symbol: 'mAAPL';
      token: 'daodiseo1...'; // Daodiseoswap token contract
      lpToken: 'daodiseo1...'; // Daodiseoswap LP token contract
      pair: 'daodiseo1...'; // Daodiseoswap pair contract against UST
    },
    ...
  }
});
```

### Query

The `Mirror` object contains contract queries for all of the Mirror core contracts, which it will run against the provided `LCDClient.`

```typescript
async function main() {
  const result = await mirror.factory.getConfig();
}

main().catch(console.error);
```

Each contract has various query operations, which you can discover in the reference [API documentation](https://daodiseomirror.github.io/mirror.js/).

### Executing

The `Mirror` object contains functions for generating proper `MsgExecuteContract` messages to be included in a transaction and broadcasted.

```typescript
const wallet = mirror.lcd.wallet(mirror.key);

async function sendMIR() {
  const tx = await wallet.createAndSignTx({
    msgs: [mirror.mirrorToken.transfer('daodiseo1...', 1_000_000)],
    fee: new StdFee(200_000, { uluna: 20_000_000 })
  });
  return await mirror.lcd.tx.broadcast(tx);
}

async function stakeLPTokens() {
  const { mAAPL } = mirror.assets;
  const tx = await wallet.createAndSignTx({
    msgs: [
      mirror.staking.bond(
        mAAPL.pair.contractAddress,
        500_000_000,
        mAAPL.lpToken
      )
    ],
    fee: new StdFee(200_000, { uluna: 20_000_000 })
  });
  return await mirror.lcd.tx.broadcast(tx);
}

async function main() {
  await sendMIR();
  await stakeLPTokens();
}

main().catch(console.error);
```

