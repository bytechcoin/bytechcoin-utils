
# BytechCoin Javascript Utilities

## Disclaimer

***Use of this code in its current state may lead to unexpected results***

This repository contains highly experimental code  with the goal of making it possible to interact with a daemon including wallet functionaity (sending/receiving transactions) without the need for `bytech-service` or `wallet-api` using Node.js. By using the code in this repo, you understand that some functions may not work, others may work but be untested, while others may upset you.

The best way to address such situations is to submit a Pull Request to resolve the issue you're running into.

## Installation

```bash
npm i git+https://github.com/bytechcoin/bytechcoin-utils
```

## Initialization

### JavaScript

```javascript
const BytechCoinUtils = require('bytechcoin-utils').CryptoNote
const coinUtils = new BytechCoinUtils()
```

### TypeScript

```typescript
import { CryptoNote } from 'bytechcoin-utils'
const coinUtils = new CryptoNote()
```

You can find TypeScript type definitions [here](index.d.ts)

### Forking

If initializing for a different CryptoNote project you can specify the configuration.
All parameters are optional.

```javascript
const BytechCoinUtils = require('bytechcoin-utils').CryptoNote
const coinUtils = new BytechCoinUtils({
  /* The amount of decimals your coin has */
  coinUnitPlaces: 8,

  /* Your address prefix - this can be found in CryptoNoteConfig */
  addressPrefix: 6581243850,

  /* The amount of keccak iterations to be performed when creating seeds and
     addresses. Should be a large number if your are supplying poor entropy
     to the createNewSeed / createNewAddress functions. */
  keccakIterations: 100,

  /* The default network fee to use in atomic units */
  defaultNetworkFee: 10
})
```

## Public Methods

#### createNewSeed([entropy], [iterations])

Creates a new address seed using the provided entropy or if entropy is undefined, uses a randomly selected entropy source.

#### createNewAddress([entropy], [language], [addressPrefix])

Creates a new [address](#address) using the provided entropy, language (for the mnemonic), and address prefix if supplied.

#### createAddressFromSeed(seed, [language], [addressPrefix])

Creates a new [address](#address) using the provided seed, language (for the mnemonic), and address prefix if supplied.

#### createAddressFromMnemonic(mnemonic, [language], [addressPrefix])

Creates a new [address](#address) using the provided mnemonic, language (for the mnemonic), and address prefix if supplied.

#### createAddressFromKeys(privateSpendKey, privateViewKey, [addressPrefix])

Creates a new [address](#address) using the provided private spend key, private view key, and address prefix if supplied.

#### decodeAddressPrefix(address)

Decodes the [address prefix](#decoded-address-prefix) from the specified CryptoNote public address.

#### decodeAddress(address, [addressPrefix])

Decodes the address into the public key pairs, prefix, payment ID, etc. Returns a [decoded address](#decoded-address) object.

#### encodeRawAddress(rawAddress)

Encodes the rawAddress using CN-Base58 encoding.

#### encodeAddress(publicViewKey, publicSpendKey, [paymentId], [addressPrefix])

Encodes the publicViewKey, publicSpendKey, and payment ID into a standard CryptoNote address (or Integrated address if payment ID is supplied)

#### createIntegratedAddress(address, paymentId, [addressPrefix])

Creates an Integrated Address using the supplied address and payment ID.

#### privateKeyToPublicKey(privateKey)

Gets the corresponding private key from the given public key.

#### scanTransactionOutputs(transactionPublicKey, outputs, privateViewKey, publicSpendKey, [privateSpendKey])

*Documentation In Progress*

#### isOurTransactionOutput(transactionPublicKey, output, privateViewKey, publicSpendKey, [privateSpendKey])

*Documentation In Progress*

#### generateKeyImage(transactionPublicKey, privateViewKey, publicSpendKey, privateSpendKey, outputIndex)

*Documentation In Progress*

#### generateKeyImagePrimative(publicSpendKey, privateSpendKey, outputIndex, derivation)

The same as generateKeyImage, but allows you to reuse a derivation you have previously created, instead of re-deriving it. Returns [keyImage, privateEphemeral].

#### createTransaction(transfers, ourOutputs, randomOuts, mixin, feeAmount, [paymentId], [unlockTime])

*Documentation In Progress*

#### serializeTransaction(transaction)

*Documentation In Progress*

#### generateKeyDerivation(transactionPublicKey, privateViewKey)

Creates the key 'derivation' given a transaction public key, and the private view key.
Can then be supplied to underivePublicKey, to determine if the transaction output belongs to you.
Returns a string.

#### underivePublicKey(derivation, outputIndex, outputKey)

Given the output index in the transaction, and the outputs key, along with a
derivation from `generateKeyDerivation`, this method will return a public spend key.
If the public spend key matches your public spend key, the transaction output is yours.
Returns a string.

## Common Data Structures

#### Address

```javascript
{
  spend: {
    privateKey: 'b74179790236324c818e40b12bd7b65fd5d09d4e5587a9f21d2c883ae6523106',
    publicKey: '0b5a6ed3fde5470fe13a6e817b5b4caf4ce014e65155ae2f6db66a3b9ad6e819'
  },
  view: {
    privateKey: 'd7a8258bd183def9b7a70b80d09784d9a04643112378ea06ef04a02d18b29805',
    publicKey: '1909af618bef1ecd30339fe12fb133bbaad05cc0ea365caf02254a5f3ae735df'
  },
  address: 'bYTCv1yzo6v37EHia82e4ATVYwU3g9wVqXGtX5CT5NRigZuqR45DwFZfTbGDWhBNBgAeszb3rs2tqgygXADjn1xy3CRGeR6vZNc',
  mnemonic: 'excess beyond nocturnal popular demonstrate lettuce asylum tissue muddy ambush inkling smelting trash sake anxiety typist sailor quick remedy cabin hamburger motherly tell topic sailor',
  seed: '517f0d3c5438416adad752557fdf001acfea189d35af8bba326c86928cc6100e'
}
```

#### Decoded Address

```javascript
{ publicViewKey: 'f0ba225065e1b9c2e43165b3e41f10fcb768853126dfa7e612a3df2deb332492',
  publicSpendKey: 'f71e440f9a5aab08dbdab0f4f36bba813660a0600f109b1371dc53be33f23c99',
  paymentId: '',
  encodedPrefix: '9df6ee01',
  prefix: 3914525,
  rawAddress: '9df6ee01f71e440f9a5aab08dbdab0f4f36bba813660a0600f109b1371dc53be33f23c99f0ba225065e1b9c2e43165b3e41f10fcb768853126dfa7e612a3df2deb332492cc073a66' }
```

#### Decoded Address Prefix

```javascript
{ prefix: '9df6ee01',
  base58: 'bYTC',
  decimal: 3914525,
  hexadecimal: '3bbb1d' }
```

### Credits

Special thanks goes out to:

* Lucas Jones
* Paul Shapiro
* Luigi111
* [The MyMonero Project](https://github.com/mymonero/mymonero-app-js)
* The Masari Project: [gnock](https://github.com/gnock)
* The Plentum Project: [DaveLong](https://github.com/DaveLong)
* The TurtleCoin Project: (https://github.com/turtlecoin)
