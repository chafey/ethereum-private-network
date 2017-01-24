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
accounts with the listed ether:

```
0xf79e502ffdc85c91e643f61eebcadecadd7330e0, 20
0x1d8a14344df5b8f96f659c965614f623df83d5e9, 20
0x22fb800aaeab6af13e8fd76623b6acab3ee15b62, 20
0x428a0e1c7f7e90dc9b4c459116776d9070fb62d3, 20
0xcd7bf72ae6bc793e4f8418490be4be0b4ee900b8, 20
0xe0f95e84a680da21b9d410d59553c91aff515104, 20
0x4529a7ad0d4979fcb3e8e577339fac51d46afc73, 0
0xa53cfb5697f17b97e36db9eb76faa2d8868f0ecf, 0
```

All account have the password 'changeme'.

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

0xf79e502ffdc85c91e643f61eebcadecadd7330e0

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

> personal.unlockAccount(from, 'changeme')

> web3.eth.getBalance(from)

20000000000000000000

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

19999579999999999999

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
