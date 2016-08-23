## Tutorial: using Web3js to interact with TestRPC 

Now that we are able to simulate a local node with [TestRPC](./testrpc.md), we can look at how to communicate with it. To do so we'll use Web3.js, which gives us a familiar javascript interface to our ethereum node, with the JSON RPC.

## Access Web3js in the Terminal

To quickly get started using the Web3.js library, we'll get it running in the terminal, and accessible from the Node.js REPL.

**Install Web3.js**
`npm install web3`

Create a file named `web3repl.js` and paste in this code:

```
const repl = require('repl');

var Web3 = require('web3');
var web3 = new Web3();

repl.start('> ').context.web3 = web3;
```

Then run it in your terminal:

```
$ node web3repl.js
> web3 // output tells us web3 is here with us
```


## Interacting with TestRPC 

Now with [TestRPC](./testrpc.md) running, we can quickly begin interfacing with in the terminal by running web3 in nodejs. 

**Set the provider:** 

`>  web3.setProvider(new web3.providers.HttpProvider('http://localhost:8545'));`

Try making a few requests corresponding to the ones we made using curl in the TestRPC tutorial:

**eth_coinbase**

Use this to get your node's primary address, this is also the first account listed when you start TestRPC in the terminal.

```
> var coinbase = web3.eth.coinbase
> coinbase;
'0x145fd656b2e2edbf01037ec8585ea40be3939b9b'
```

**eth_getBalance**

Get your coinbase address's balance in the `latest` block

```
> web3.eth.getBalance(coinbase)
{ [String: '2.1267647932558653671313007785132687361e+37'] s: 1, e: 37, c: [ 2126764793, 25586536713130, 7785132687361 ] }
```


**eth_getBlockByNumber**
```
> web3.eth.getBlock("latest")
{ number: 46,
  hash: '0xca98b8b2bd51a4ddc3f55992dac5d57ff977b36940c4b791d63cfd5dbb8f970c',
  parentHash: '0xf225b21082e9323f99c23a217406246f25825372e6eee8a2074868ba69d78039',
  nonce: '0x0', ......

> web3.eth.getBlock(0x02)
{ number: 2,
  hash: '0x5c16948f0a0f6be76fe5b828b4c0c1568646771045d7dedcfb23872702a6bcb9',
  parentHash: '0x0aa37a5774d7071bb5d3935c1aa055b0838ec8fe4a93eb06adbae15402e17164',
  nonce: '0x0', .........
```


# Deploy and interact with a smart contract in the browser

For this we'll make use of some small tweaks I've made to Ledger Labs' [Simple Storage]() repository. 

First, clone the repo locally, then open the `index.html` file in a text editor. Make sure testRPC is running, by default it will be `Listening on localhost:8545`, but if you've changed the port, then you'll need to update the setting on line 42: 

`web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));`












