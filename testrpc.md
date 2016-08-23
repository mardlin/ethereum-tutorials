# Tutorial: TestRPC for a local ethereum testing environment

This is intended to crystalize my own understanding of these tools. If it helps anyone else, then all the better.

## TestRPC

[TestRPC](https://github.com/ethereumjs/testrpc) simulates an ethereum node interface, without actually running a full ethereum or morden (testnet) node. 

You can interact with it using simple HTTP requests, using the [JSON RPC API](https://github.com/ethereum/wiki/wiki/JSON-RPC). 

### What is RPC?

RPC stands for **Remote Procedure Call**. JSON-RPC provides a standard specification for calling a procedure from a remote (or local) machine, without specifying the transport protocol (ie. sockets, HTTP). You can read the [specification](http://www.jsonrpc.org/specification), or a more friendly overview [here](https://www.cs.cf.ac.uk/Dave/C/node33.html). 

Basically, RPC is a way to say to an Ethereum node: "*Please run (some method), with (some  arguments)*", and to write it in such a way that your application can communicate via HTTP with any of the major ethereum client implementations.  If you're familiar with REST APIs, you should be comfortable with JSON-RPC. 


**For example:** To get the balance of an account, we'd send an `eth_getBalance` method request: 

```
{
  "method": "eth_getBalance",
  "params": [
    "0x407d73d8a49eeb85d32cf465507dd71d507100c1",
    "latest"
  ],
  "jsonrpc": "2.0", //required to specify the protocol version
  "id": 1 // this will be included on the response, and used to match
}
```

which would return this response:

```
{
  "result": "0x0234c8a3397aab58",
  "jsonrpc": "2.0",
  "id":1
}
```


The `id` and `jsonrpc` properties are explained [in the specification](http://www.jsonrpc.org/specification#request_object).

## Running and interacting with TestRPC

Once it's installed, simply running `$ testrpc` will make it available at `http://localhost:8545`, where you can then send HTTP requests from a browser, or just via curl on the command line (see below). 

### Interesting options for TestRPC: 

`--account="<privatekey>,balance"`: Specify a private key, and the balance that should belong to that account. 

`-b` or `--blocktime`: By default, testrpc will only mine a new block when there is a transaction to mine. Setting this option will change that behavior.

`--debug`: this outputs opcodes from the simulated Ethereum Virtual Machine (EVM. Try deploying a contract, or sending a transaction to see this in action. 


### Interacting with the TestRPC 

TestRPC provides most of the RPC methods defined in the Ethereum wiki

- [TestRPC implemented methods](https://github.com/ethereumjs/testrpc#implemented-methods)
- [Ethereum JSON-RPC wiki](https://github.com/ethereum/wiki/wiki/JSON-RPC). 


The wiki documentation shows extra JSON properties:

`$ curl localhost:8545  -X POST --data '{"jsonrpc":"2.0","method":"eth_coinbase","params":[],"id":64}'`

You'll definitely want to include thse for application programming, but for now we can get away with just the RPC method:

`$ curl localhost:8545  -X POST --data '{"method":"eth_coinbase"}'`


Refer to the [wiki](https://github.com/ethereum/wiki/wiki/JSON-RPC#web3_clientversion) for more example requests.

**eth_coinbase**

Use this to get your node's primary address, this is also the first account listed when you start TestRPC in the terminal.

```
$ curl localhost:8545 -X POST --data '{"method":"eth_coinbase","jsonrpc":"2.0","id":2}'

{"result":"0x1644fbd03497c1c3f7742107b58ec0a0bea46c4a"} 
```

**eth_getBalance**

Get your coinbase address's balance in the `latest` block

```
$ curl localhost:8545 -X POST --data '{"method":"eth_getBalance","params":["0x1644fbd03497c1c3f7742107b58ec0a0bea46c4a", "latest"],"jsonrpc":"2.0","id":2}'

{"id":1,"jsonrpc":"2.0","result":"0x0ffffffffffffff00000000000000001"}
```


**eth_getBlockByNumber**

Get a specific block by it's number (specified in hex), or using "latest", "earliest" or "pending".

```
$ curl localhost:8545 -X POST --data 
  '{"method":"eth_getBlockByNumber", 
    "params": [
   "latest",
   true
  ]}'

{"result":{"number":"0xcb","hash":"0xa9651a419e2593635f8c20221fbf93992dfc1624448f75ccb862b060d80686f7","parentHash":"0x05a31dcb1bfe5a53e906da807c36d88efe894da300603cd2d4c8191cddb60aef","nonce":"0x0","sha3Uncles":"0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347","logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xf9a75c22fdeae586f7573b703d78be5f12f07f053f68816b832f35039a9c5150","receiptRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","miner":"0x0000000000000000000000000000000000000000","difficulty":"0x0","totalDifficulty":"0x0","extraData":"0x0","size":"0x03e8","gasLimit":"0x47e7c4","gasUsed":"0x0","timestamp":"0x57bc32f7","transactions":[],"uncles":[]}}
```

Try out more requests using the full list of [Ethereum JSON-RPC methods](https://github.com/ethereum/wiki/wiki/JSON-RPC#json-rpc-methods).
