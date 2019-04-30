---
title: "ubuntu18.04 架设以太坊私有链"
date: 2019-04-06T:21:01+08:00
draft: false
---

## 安装 geth, solc

```shell
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
sudo apt-get install solc
```

## 创世块设置
```shell
mkdir -p ~/eth/data
cd eth
vi genesis.json
```
输入

```json
{
"config": {
        "chainId": 123456,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
},
"nonce": "0x0000000000000042",
"difficulty": "0x020000",
"mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
"coinbase": "0x0000000000000000000000000000000000000000",
"timestamp": "0x00",
"parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
"extraData": "0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa",
"gasLimit": "0x4c4b40",
"alloc": {}
}
```

### 初始化
```shell
geth --datadir data init genesis.json
```

## 启动
```shell
geth --datadir data --networkid 123456 --rpc --rpccorsdomain "*" --nodiscover console
```
进入 javascript console

## Geth javascript 控制台 commandline 
```shell
web3		# 列出web3 对象的接口

personal.newAccount()	# 创建新账号
personal.unlockAccount(eth.accounts[0])
admin.nodeInfo() 		# 查看节点信息

miner.start(1)			# 
miner.stop()			# 
miner.setEtherbase(eth.accounts[1])

eth.coinbase 			# 当前默认矿工账号
eth.accounts
eth.account[0]
eth.getBalance(eth.accounts[0])
eth.sendTransaction({
		from: eth.accounts[0],
		to: "0x009998877787...",
		value: web3.toWei(3, 'ether')
	})
eth.blockNumber
eth.getBlock(1)
eth.getTransaction("0x34535...")

web3.fromWei(eth.getBalance(eth.accounts[0]), 'ether')

txpool.status 		# 查看交易状态

```
## 连接到其他节点

可以通过admin.addPeer()方法连接到其他节点，两个节点要想联通，必须保证网络是相通的，并且要指定相同的networkid。

假设有两个节点：节点一和节点二，networkid都是1108，通过下面的步骤就可以从节点一连接到节点二。

首先要知道节点二的enode信息，在节点二的js console中执行下面的命令查看enode信息

```shell
> admin.nodeInfo.enode
"enode://9e86289ea859ca041f235aed87a091d0cd594b377cbe13e1c5f5a08a8a280e62d4019ac54063ed6a1d0e3c3eaedad0b73c40b99a16a176993f0373ffe92be672@[::]:30304"

> admin.addPeer("enode://9e86289ea859ca041f235aed87a091d0cd594b377cbe13e1c5f5a08a8a280e62d4019ac54063ed6a1d0e3c3eaedad0b73c40b99a16a176993f0373ffe92be672@127.0.0.1:30304")
```
addPeer()的参数就是节点二的enode信息，注意要把enode中的[::]替换成节点二的IP地址。连接成功后，节点二就会开始同步节点一的区块，同步完成后，任意一个节点开始挖矿，另一个节点会自动同步区块，向任意一个节点发送交易，另一个节点也会收到该笔交易。

通过admin.peers可以查看连接到的其他节点信息，通过net.peerCount可以查看已连接到的节点数量。

除了上面的方法，也可以在启动节点的时候指定--bootnodes选项连接到其他节点。
