# ethereum-private-network
Instructions on how to setup an ethereum private test network for development.
This was done with Mac OS X.

Pre-requisities
---------------

* [homebrew](http://brew.sh/)

Install Geth
------------

[Geth](https://github.com/ethereum/go-ethereum/wiki/geth) is the command line
interface for running a full ethereum node implemented in Go.

> brew update

> brew upgrade

> brew tap ethereum/ethereum

> brew install ethereum

Initialize local test network
-----------------------------

The first thing you need to do is initialize your ethereum test network:

> bin/init

This script will clear/reset your ethereum blockchain and create the following
accounts:

``` javascript
[{
  address: '0x5DFE021F45f00Ae83B0aA963bE44A1310a782fCC',
  ether: 200000,
  privateKey: '0xf059416a2f6bb05d0770bbacb24a6430757aa7db5c15959838ed142b486df5b8',
  hasWallet: true,
  password: 'iloveethereum',
  coinbase: true
},
{
  address: '0xFE2b768a23948EDDD7D7Caea55bAa31E39045382',
  ether: 10,
  privateKey: '0xb32e083f4ce24956c2989d7e0c57bfd6759b3fa7ba8730c342e12fdf4ba8deb6',
  hasWallet: true,
  password: 'iloveethereum'
},
{
  address: '0xA9a418dA22532Bd1189fF8Be5Cdaf3570bF9da43',
  ether: 20,
  privateKey: '0x5fb75cc4852a0340e6bb061d039974809d36e6974ff849d3b3586909b65829d8',
  hasWallet: true,
  password: 'iloveethereum'
},{
  address: '0x9F3A4BBeD4660F2DCCd6E980e2FaA6d6214E5Dc8',
  ether: 30,
  privateKey: '0x6ac723a720aa0efa5939a8526ec98dbc1bc6f81146b24fdf9f3e1d09a993c222',
  hasWallet: true,
  password: 'iloveethereum'
},{
  address: '0xc10261166b4699D3c1535Aa30AC29446c755f065',
  ether: 40,
  privateKey: '0x9160633fbdb0bf99f444a3c6018d8a6e4a9a353f0745bb56e43025648118dc27',
  hasWallet: true,
  password: 'iloveethereum'
},{
  address: '0xe480219e1904de4500Cd8459C74d388457A3f3Ec',
  ether: 0,
  privateKey: '0xba774bc82c19dee108f3da59f33aabcec8155f6d102adce371186fec839d50ef',
  hasWallet: true,
  password: 'iloveethereum'
},
},{
  address: '0xED7211F84b37B0f62d345462fFeB56b57B787539',
  ether: 0,
  privateKey: '0xb34f18662b6d09bc01b2b26e3d325b03f824cb10be8226ad65b1f4cac457eb73',
  hasWallet: true,
  password: 'iloveethereum'

},{
  address: '0xCC52165260FB50dA8Fc9fEd714e33884D324f7Dd',
  ether: 70,
  privateKey: '0x1de8d3b032083b477f623d4fc004a120459ed17688aa5311c7fbcf30a21ec0e4',
  hasWallet: false
}
},{
  address: '0x28c36458566E89b3F9F3D5c1Ba52FDF840072598',
  ether: 0,
  privateKey: '0x26fd4b1dbbea7c356b4ae3b32d617fa7df301f3ed26873dcc90cd86862867bcd',
  hasWallet: false
}
```




Start ethereum mining node
--------------------------

The ethereum network needs a mining node to process transactions:

> bin/mine

The first time you run geth on your machine, it will generate a DAG.  This can
take several minutes depending upon the speed of your CPU.  Once it finishes
generating the DAG, it will start mining and generating messages like this:

```
I0124 14:41:07.325501 miner/unconfirmed.go:83] 🔨  mined potential block #1 [f60eb249…], waiting for 5 blocks to confirm
I0124 14:41:07.325835 miner/worker.go:516] commit new work on block 2 with 0 txs & 0 uncles. Took 225.475µs
```

The mining node deposits ethereum into the following account:

0x5DFE021F45f00Ae83B0aA963bE44A1310a782fCC

Attach to your mining node
--------------------------

You can interact with the ethereum network by attaching to your mining node:

> bin/attach

This will present a javascript console where you can run various commands

Send ether from one account to another
--------------------------------------

Form the console attached to your mining node, you can send ether from one
 account to another.  First we check the balances of the from and to accounts:

> var from = web3.eth.accounts[1]

> personal.unlockAccount(from, 'iloveethereum')

> web3.eth.getBalance(from)

10000000000000000000

> var to = web3.eth.accounts[2]

> web3.eth.getBalance(to)

20000000000000000000

Now we send ether:

> eth.sendTransaction({from: from, to: to, value: 1})

"0xdc6a6858f57c100398dabb5868549a6a113508b40bcc43b5b5aad639821c3fad"

If you check the mining node console, you will see this transaction logged:

```
I0124 09:35:24.849818 internal/ethapi/api.go:1047]
Tx(0xdc6a6858f57c100398dabb5868549a6a113508b40bcc43b5b5aad639821c3fad)
to: 0x22fb800aaeab6af13e8fd76623b6acab3ee15b62
```

Now you can check the balances of the from and to account to verify that
ether was moved:

> web3.eth.getBalance(from)

9999579999999999999

NOTE: The reason more than 1 is gone from the ether balance is because it costs
ether to execute the send ether transaction.  This transaction fee is given to
the miner.

> web3.eth.getBalance(to)

20000000000000000001

Deploy the greeter contract and call the greet method
-----------------------------------------------------

From the attached console, you can deploy the
[greeter smart contract](https://www.ethereum.org/greeter) and
use it:

> loadScript('deploy.js');

```
Contract mined! address: 0x1b9dfd2ea79f59491b7881c508010af5410cd096 transactionHash: 0xe92dfdbea562e59661a483f21b8d2fedebebe4eb0b9cee00bfc9fb97ffa329e6
null [object Object]
Contract mined! address: 0xe9f5d76475b180a709025dde31091b9793742119 transactionHash: 0x1ca2b4240d5957ea68ff9dc5aeee08050b8d533e1636586e47ad9222b945b1f1
```

> greeter.greet();

"hello private test network"

NOTE: The contract in deploy.js is the default contract displayed in the
[online solidity compiler](https://ethereum.github.io/browser-solidity)
