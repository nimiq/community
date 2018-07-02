---
layout: post
section_id: academy
title:  "How to send NIM with the Node.js API"
date:   2018-05-07 08:00:00
excerpt: The basics of the Nimiq API
type: lesson
author: Smitop
images:
  - "/images/academy/sendnimnodejs.jpg"
---

## Installing the Nimiq API
If you want to use the Nimiq API in your own projects, you'll first need to include the Nimiq API. Let's look at how to do that in Node.js.

First, you'll need to [install the Nimiq compiled binary](https://nimiq.com/#downloads) on you computer. Next, open a new Node.js file. Put this in it:
```js
const Nimiq = require("/usr/share/nimiq/app/lib/node.js"); // For Ubuntu/Debian
const Nimiq = require("C:\\Program Files\\Nimiq\\lib\\node.js");  // For Windows
```

Alternatively, you can install it with ``npm``.
First, run ``npm install @nimiq/core`` in the directory of the project. Then, do 
```js
const Nimiq = require('@nimiq/core');
```

If you get errors running this, make sure you have updated Node.js. If you don't get any errors running this program, then it worked!

From now on, I'll assume you've ``require``d the API.

## Sending the NIM
The first thing you'll want to do, is specify what network you want to connect to. The mainnet, or "normal" network is identified as "main". The testnet gets "test". The network for testing for security bugs gets "bounty". And finally, the network used by the Nimiq developers for testing, is identified by "dev". For this example, let's use the mainnet.
```js
Nimiq.GenesisConfig.main();
```
Many functions in the Nimiq library take time to complete, and use ``Promise``s. To handle this, we'll need to put our code in an async function.
```js
Nimiq.GenesisConfig.main();
async function nimiqCode() {
}
nimiqCode();
```
Next, let's connect to the Nimiq network. Nimiq supports 3 different types of nodes, which are "full", "light", and "nano". Full nodes sync the entire blockchain. Light nodes sync less of the blockchain, but still have enough information to get the balance of any account on the network. Nano clients are the lightest, and need to query a full, or light node that it's connected to, for almost everything. However, nano nodes have a reduced memory footprint, and require much less syncing. For this example, we'll use a nano node.
```js
Nimiq.GenesisConfig.main();
async function nimiqCode() {
  const consensus = await Nimiq.Consensus.nano();
  consensus.network.connect();
}
nimiqCode();
```
Next, let's import our private key. It's recommended that you create a different wallet for scripting purposes, so a bug doesn't wipe out your entire wallet. You'll need your 24 words for this next step. To parse a 24 word key, you'll need the MnemonicPhrase library. Put [the library](https://raw.githubusercontent.com/nimiq/mnemonic-phrase/master/mnemonic-phrase.es5.min.js) in a file named ``phrases.js``, in the same directory of the current code you have, and add this to the top:
```js
const MnemonicPhrase = require("./phrases.js");
Nimiq.GenesisConfig.main();
async function nimiqCode() {
  const consensus = await Nimiq.Consensus.nano();
  consensus.network.connect();
}
nimiqCode();
```
Now, let's parse it. We'll use the private key to derive a KeyPair, and then we'll create a wallet from that.
```js
const MnemonicPhrase = require("./phrases.js");
Nimiq.GenesisConfig.main();
async function nimiqCode() {
  const consensus = await Nimiq.Consensus.nano();
  consensus.network.connect();
  const privateKey = Buffer.from(MnemonicPhrase.mnemonicToKey(
  "PUT YOUR PRIVATE KEY HERE"
  )), "hex");
  const key = new Nimiq.PrivateKey(privateKey);
  const keyPair = Nimiq.KeyPair.derive(key);
  const wallet = new Nimiq.Wallet(keyPair);
}
nimiqCode();
```
Now, let's create a transaction object, and tell a node we're connected to to send the transaction to the network. This will send my wallet 0.2 NIM, with a 0.00140 NIM transaction fee:
```js
const MnemonicPhrase = require("./phrases.js");
Nimiq.GenesisConfig.main();
async function nimiqCode() {
  const consensus = await Nimiq.Consensus.nano();
  consensus.network.connect();
  const privateKey = Buffer.from(MnemonicPhrase.mnemonicToKey(
  "PUT YOUR PRIVATE KEY HERE"
  )), "hex");
  const key = new Nimiq.PrivateKey(privateKey);
  const keyPair = Nimiq.KeyPair.derive(key);
  const wallet = new Nimiq.Wallet(keyPair);

  var transaction = wallet.createTransaction(
  Nimiq.Address.fromUserFriendlyAddress("NQ92 589S 4CN6 U0FX NQRV NHQP TQNV CF1U BVHU"),
  , 20000, 140, consensus.blockchain.head.height);
  await consensus.relayTransaction(transaction);
}
nimiqCode();
```
