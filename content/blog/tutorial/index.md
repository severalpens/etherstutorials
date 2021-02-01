---
title: Ethers tutorial
date: "2021-02-02T22:40:32.169Z"
description: A tutorial about Ethers.js.
---
In this video I'm going to go through Ethers.js and show how to use it using a Node.js console application.

Before you start you'll need an Infura Project ID and secret. So sign up there and create a new project.

Make sure you keep this information secret. So I created this project for demo purposes and I'll delete it before posting this tutorial

Don't touch any settings below here. Just creating the project should be all you need to do here.

[Show Infura website]

Starting off, this is a minimal node.js application with index.js as the entry point. 
I've installed these packages: ethers and dotenv.

[Show package.json]

Once installed, I'll use the console app to generate a random ethereum blockchain address:

 ```javascript
require('dotenv').config();
const ethers = require('ethers');
const randomAccountRaw = ethers.Wallet.createRandom();
console.log(randomAccountRaw);

```
```javascript
console.log('To be added to .env file:');
console.log(`account1_address=${tmp.address}`);
console.log(`account1_privateKey=${tmp.privateKey}`);
```
I'll save the address to dotenv.

Now to check its balance I'll need to connect via my Infura account

To do that I've added in these two lines:

The provider in my connection to the rinkeby testnet blockchain via infrua.
and A wallet object is created by passing in my account's private key and the provider object.

 ```javascript
const provider = new ethers.providers.InfuraProvider('rinkeby', process.env.INFURA);
```


 ```javascript
 //IIFE to handle async/await
 (async function () {
  const balanceRaw = await provider.getBalance(process.env.account1_address);
  console.log(balanceRaw);
  const balanceInWei = parseInt(balanceRaw._hex);
  console.log(balanceInWei);
	const ethBalance = balanceInWei * 0.000000000000000001;
	const result = Math.round(ethBalance * 100) / 100;
	console.log(result);
})();
 ```


And as this shows there is a zero balance on the rinkeby testnet for this account.
To get some monopoly money to play with on the rinkeby testnet go to https://faucet.rinkeby.io/ and follow the instructions.
 
 [show faucet page]

Once you have some fake ether you should see the balance when you re-run the script

[rerun balance script]

Next is to transfer between two addresses so I'll create another random address and transfer some ETH value:

 ```javascript
require('dotenv').config();
const ethers = require('ethers');
const provider = new ethers.providers.InfuraProvider('rinkeby', process.env.INFURA);
const wallet = new ethers.Wallet(process.env.privateKey, provider);
//IIFE to handle async/await
(async function () {
	const balanceInWei = await this.provider.getBalance(address);
	const wei = parseInt(balanceInWei._hex);
	const eth = wei * 0.000000000000000001;
	const result = Math.round(eth * 100) / 100;
	console.log(result);
})();
```
Next is create and run a simple contract. So I'll go to Remix and create a simple contract using Solidity language.
To learn Solidity go to solidity.com and hone your skills using remix.

[show solidity & remix]

Here's a Foo/Bar/Baz contract example and also here's an ERC20 example.

```solidity
pragma solidity ^0.6.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v3.3.0/contracts/token/ERC20/ERC20.sol";

contract BasicToken is ERC20 {
  constructor(uint256 initialBalance) ERC20("Basic", "BSC") public {
    _mint(msg.sender, initialBalance);
    _setupDecimals(0);
  }

}
```

In remix you can compile and deploy these contracts on Rinkeby as long as you've got the Metamask browser extension installed and an account with Rinkeby ETH.  

 [compile & deploy contracts in Remix]
 
 Get the addresses and save them somewhere. They're just addresses so don't have to be kept secure but for convenience I'm going to store them in the .env file. 
 
 [add contract addresses to .env]
 
 Now we have the ERC20 contract deployed I can demonstrate the transfer of tokens:
 
 ```javascript
 [TBA transfer of tokens]
 ```

Lastly, instead of relying on Remix I'll show how to compile and deploy locally.

First we'll install waffle to compile locally so we can work directly in the node.js project instead of remix

[install ethereum-waffle]

Add a waffle config file, create a contracts folder, add the contract then run the command to compile contracts:

```
waffle compile
```

After this there should be a compiled contracts folder AKA an artifacts folder that will contain amongst other things the abi and bytecode.

Now that we have the artifacts we can programmatically deploy the contracts. Let start with the FooBar contract to check that it works.

 ```javascript
require('dotenv').config();
const ethers = require('ethers');
const provider = new ethers.providers.InfuraProvider('rinkeby', process.env.INFURA);
const wallet = new ethers.Wallet(process.env.privateKey, provider);
const fooBarArtifacts = require('./artifacts/foobar');
const erc20Artifacts = require('./artifacts/basictoken');

//IIFE to handle async/await
(async function () {
  let contract = require;
  let bytecode = JSON.parse(fooBarArtifacts.bytecode);
  let provider = new ethers.providers.InfuraProvider('rinkeby',process.env.INFURA);
  let wallet = new ethers.Wallet(process.env.privateKey, provider);
  let factory = new ethers.ContractFactory(fooBarArtifacts.abi, fooBarArtifacts.bytecode, wallet);
 // let args = deploy.inputs.map(x => x.value);
 // let tx = await factory.deploy(...args);
  await tx.deployed();
  console.log(tx);
})();
```

Put the contract address into the .env file. 

Repeat this process for the ERC20 Basic Token.

 ```javascript
require('dotenv').config();
const ethers = require('ethers');
const provider = new ethers.providers.InfuraProvider('rinkeby', process.env.INFURA);
const wallet = new ethers.Wallet(process.env.privateKey, provider);
const fooBarArtifacts = require('./artifacts/foobar');
const erc20Artifacts = require('./artifacts/basictoken');

//IIFE to handle async/await
(async function () {
  let contract = require;
  let bytecode = JSON.parse(fooBarArtifacts.bytecode);
  let provider = new ethers.providers.InfuraProvider('rinkeby',process.env.INFURA);
  let wallet = new ethers.Wallet(process.env.privateKey, provider);
  let factory = new ethers.ContractFactory(fooBarArtifacts.abi, fooBarArtifacts.bytecode, wallet);
 // let args = deploy.inputs.map(x => x.value);
 // let tx = await factory.deploy(...args);
  await tx.deployed();
  console.log(tx);
})();
```








