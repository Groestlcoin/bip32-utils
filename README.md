# bip32grs-utils

A set of utilities for working with BIP32.
Compatible with groestlcoinjs-lib `^3.0.0` and `^4.0.0`.


## Example

#### BIP32 Account
``` javascript
const bip39 = require('bip39')
const bitcoin = require('groestlcoinjs-lib')
const bip32utils = require('bip32grs-utils')

let mnemonic = bip39.generateMnemonic()
let seed = bip39.mnemonicToSeedSync(mnemonic)

let hdNode = bitcoin.bip32.fromSeed(seed)

let childNode = hdNode.deriveHardened(0)
let external = childNode.derive(0)
let internal = childNode.derive(1)
let account = new bip32utils.Account([
  new bip32utils.Chain(external.neutered()),
  new bip32utils.Chain(internal.neutered())
])

console.log(account.getChainAddress(0))
// => FtQSUR8aiRe1SUHMotkpHSnxzf8ndsktdd

account.nextChainAddress(0)

console.log(account.getChainAddress(1))
// => FVNt9vrQ3rni4MAuxHpz1GWZXbQTKjeF9b

console.log(account.nextChainAddress(1))
// => Fgh2nwkR3YSJHtngHeNwvBgbMJmokU1buK

console.log(account.derive('FtQSUR8aiRe1SUHMotkpHSnxzf8ndsktdd'))
// => xpub6A5Fz4JZg4kd8pLTTaMBKsvVgzRBrvai6ChoxWNTtYQ3UDVG1VyAWQqi6SNqkpsfsx9F8pRqwtKUbU4j4gqpuN2gpgQs4DiJxsJQvUW9uDs

// NOTE: passing in the parent nodes allows for private key escalation (see xprv vs xpub)

console.log(account.derive('FtQSUR8aiRe1SUHMotkpHSnxzf8ndsktdd', [external, internal]))
// => xprv9vodQPEygdPGUWeKUVNd6M2N533PvEYP21tYxznauyhrYBBCmdKxRJzmnsTsSNqfTJPrDF98GbLCm6xRnjceZ238Qkf5GQGHk79CrEtBmy6
```


#### BIP32 Chains
``` javascript
const bip39 = require('bip39')
const bitcoin = require('groestlcoinjs-lib')
const bip32utils = require('bip32grs-utils')

let mnemonic = bip39.generateMnemonic()
let seed = bip39.mnemonicToSeedSync(mnemonic)

let hdNode = bitcoin.bip32.fromSeed(seed)

let chain = new bip32utils.Chain(hdNode)

let address = chain.get()
console.log(chain.find(address))
// => 0

for (let k = 0; k < 10; ++k) chain.next()

address = chain.get()

console.log(chain.find(address))
// => 10

console.log(chain.pop())
// => pops the 11th address
```


## LICENSE [MIT](LICENSE)
