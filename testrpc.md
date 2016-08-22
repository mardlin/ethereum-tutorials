# Tutorial: TestRPC for a local ethereum testing environment

This is intended to crystalize my own understanding of these tools. If it helps anyone else, then all the better.

## TestRPC

[TestRPC](https://github.com/ethereumjs/testrpc) simulates an ethereum node interface, without actually running a full ethereum or morden (testnet) node. 

You can interact with it using simple HTTP requests, using the [JSON RPC API](https://github.com/ethereum/wiki/wiki/JSON-RPC). 

### What is RPC?

RPC means **remote procedure call**, and there's a good explanation [here](https://www.cs.cf.ac.uk/Dave/C/node33.html). 

Basically, RPC is a way to say to an Ethereum node: "*Please run this {{method}}, with these {{arguments}}*". 

For example: To get the balance of an account, we'd send an `eth_getBalance` method request: 

```
{
  "jsonrpc": "2.0", //required to specify the protocol version
  "method": "eth_getBalance",
  "params": [
    "0x407d73d8a49eeb85d32cf465507dd71d507100c1",
    "latest"
  ],
  "id": 1 // this will be included on the response, and used to match
}
```

And get a response:

```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```




These properties are listed here: http://www.jsonrpc.org/specification#request_object

TestRPC provides most of the RPC methods defined in the Ethereum wiki:

- [TestRPC implemented methods](https://github.com/ethereumjs/testrpc#implemented-methods)
- [Ethereum JSON-RPC wiki(https://github.com/ethereum/wiki/wiki/JSON-RPC). 

Once it's installed, simply running `$ testrpc` will make it available at `http://localhost:8545`, where you can then send requests from a browser application, or just via curl on the command line. 

The wiki documentation shows extra JSON properties:

`$ curl localhost:8545  -X POST --data '{"jsonrpc":"2.0","method":"eth_coinbase","params":[],"id":64}'`

but you can get away with just the RPC method:

`$ curl localhost:8545  -X POST --data '{"method":"eth_coinbase"}'`


Refer to the [wiki](https://github.com/ethereum/wiki/wiki/JSON-RPC#web3_clientversion) for more example requests.


### Interesting options for TestRPC: 

`-b` or `--blocktime`: By default, testrpc will only mine a new block when there is a transaction to mine. Setting this option will change that behavior.

`--debug`: this outputs opcodes from the simulated Ethereum Virtual Machine (EVM. Try deploying a contract, or sending a transaction to see this in action. 


### Sample commands to try:

**eth_coinbase**

```
$ curl localhost:8545 -X POST --data '{"method":"eth_coinbase"}'
{"result":"0x1644fbd03497c1c3f7742107b58ec0a0bea46c4a"} # This is also the first listed account
```

**eth_sendTransaction**

```
$ curl localhost:8545 -X POST --data \
  '{"method":"eth_sendTransaction",  \
    "params": [{    \
        # use your own address  \
        "from": "0x1644fbd03497c1c3f7742107b58ec0a0bea46c4a", \ 
        "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567", \
        "gas": "0x76c0", \
        "gasPrice": "0x9184e72a000", \
        "value": "0x9184e72a", \
        "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb   8eb970870f072445675" \
    }] \
  }'
{"result":"0xe66b32aa57f75d316b413620f34efcdfe89d6d6eafec5829a58b722e161b30c2"}
```
