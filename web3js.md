## Tutorial: using Web3js to interact with TestRPC 

Now that we are able to simulate a local node with [TestRPC](./testrpc.md), we can look at how to communicate with it. To do so we'll use Web3.js, which gives us a familiar javascript interface to our ethereum node, with the JSON RPC.

**Install Web3.js**
`npm install web3`

Create a file named `web3repl.js` and paste in this code:

```
const repl = require('repl');

var Web3 = require('web3');
var web3 = new Web3();

repl.start('> ').context.web3 = web3;
```

Then run it from a terminal:

```
$ node web3repl.js
> web3 //output
```

# Deploy and interact with a smart contract in the browser

For this we'll make use of some small tweaks I've made to Ledger Labs' [Simple Storage]() repository. 

First, clone the repo locally, then open the `index.html` file in a text editor. Make sure testRPC is running, by default it will be `Listening on localhost:8545`, but if you've changed the port, then you'll need to update the setting on line 42: 

`web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));`












