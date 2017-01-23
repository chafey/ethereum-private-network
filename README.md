# ethereum-private-network
Instructions on how to setup an ethereum private test network for development.
This was done with Mac OS X.

Pre-requisities
---------------

* homebrew

Install Geth
------------

> brew update

> brew upgrade

> brew tap ethereum/ethereum

> brew install ethereum

Initialize local test network
-----------------------------

> bin/init

the local test network is initialized with the following accounts with ETH:

"0xf79e502ffdc85c91e643f61eebcadecadd7330e0", 20
"0x1d8a14344df5b8f96f659c965614f623df83d5e9", 20
"0x22fb800aaeab6af13e8fd76623b6acab3ee15b62", 20
"0x428a0e1c7f7e90dc9b4c459116776d9070fb62d3", 20
"0xcd7bf72ae6bc793e4f8418490be4be0b4ee900b8", 20
"0xe0f95e84a680da21b9d410d59553c91aff515104", 20
"0x4529a7ad0d4979fcb3e8e577339fac51d46afc73", 0
"0xa53cfb5697f17b97e36db9eb76faa2d8868f0ecf", 0


Start local ethereum mining node
--------------------------------

> bin/mine

The mining node deposits ethereum into the following account:
0xf79e502ffdc85c91e643f61eebcadecadd7330e0

Connect to the local ethereum node (in a separate tab)
------------------------------------------------------

> bin/attach

Send ether from one account to another
--------------------------------------
> var from = web3.eth.accounts[1]

> personal.unlockAcccount(from, 'changeme')

> web3.eth.getBalance(from)

20000000000000000000

> var to = web3.eth.accounts[2]

> web3.eth.getBalance(to)

20000000000000000000

> eth.sendTransaction({from: from, to: to, value: 1})

> web3.eth.getBalance(from)

19999579999999999999

> web3.eth.getBalance(to)

20000000000000000001

Deploy the greeter contract and call the greet method
-----------------------------------------------------

> loadScript('deloy.js');

Contract mined! address: 0x1b9dfd2ea79f59491b7881c508010af5410cd096 transactionHash: 0xe92dfdbea562e59661a483f21b8d2fedebebe4eb0b9cee00bfc9fb97ffa329e6
null [object Object]
Contract mined! address: 0xe9f5d76475b180a709025dde31091b9793742119 transactionHash: 0x1ca2b4240d5957ea68ff9dc5aeee08050b8d533e1636586e47ad9222b945b1f1

> greeter.greet();

"hello private test network"
