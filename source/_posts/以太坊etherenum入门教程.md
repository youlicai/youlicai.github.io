---
title: 以太坊etherenum入门教程
date: 2018-08-022 12:49:24
categories: 
- 区块链
tags: 
- 区块链
---

## Geth下载安装 ##
[下载地址](https://ethereum.github.io/go-ethereum/downloads/)

## 环境搭建 ##
我的项目目录是**H:\ETH**，同时在该目录下创建genesis.json文件，内容如下
<pre>
{
  "config": {
        "chainId": 15,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "coinbase" : "0x0000000000000000000000000000000000000000",
    "difficulty" : "0x40000",
    "extraData" : "",
    "gasLimit" : "0xffffffff",
    "nonce" : "0x0000000000000042",
    "mixhash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "timestamp" : "0x00",
    "alloc": { }
}
</pre>

genesis.json是创世区块配置文件，通过此配置让我们很容易挖矿以方便测试，具体每个字段的含义可暂时忽略！


在命令行窗口进入到**H:\ETH**目录下

### 执行创世区块的初始化 ###
命令如下：
<pre>
geth --datadir data init genesis.json
</pre>

执行结果如下：

![](https://i.imgur.com/lr8PwNT.png)

此时在ETH目录下会生成data目录，data目录又包含geth和keystore目录，geth目录存储区块数据，keystore目录则保存账户信息。


### 启动节点连接私有测试网络 ###

命令如下:

<pre>
geth --datadir data --networkid 123 --nodiscover console
</pre>

- --networkid 123参数表示区块链网络ID标识。
- --nodiscover参数表示节点私有。
- **console参数表示进入geth控制台。**

![](https://i.imgur.com/LoImR0u.png)

## Geth交互使用 ##

### 创建新账户 ###

<pre>
personal.newAccount("123")
</pre>

"123"为密码，千万不要忘记密码！输入指令，回车。

![](https://i.imgur.com/yuaNNtx.png)

绿色的字符串就是你的账户地址,同时会在keystore目录下产生这个文件：

![](https://i.imgur.com/Qa7J8BT.png)

<font color="red">为了安全，该文件建议离线存储，将keystore文件移走，保存在u盘中。打算进行转账时，将keystore文件移回到原位置即可。</font>

### 查询账户列表 ###

<pre>
eth.accounts
</pre>

### 查询账户余额 ###

<pre>
eth.getBalance(eth.accounts[0])
</pre>


### 启动或停止挖矿 ###

<pre>
miner.start();
miner.stop();
</pre>

挖到1个区块时停止挖矿，挖矿所得默认进入第一个账户里。

### 转账 ###
<pre>
eth.sendTransaction({from:"0x1e173732f1d194c95f9293f120a9b6bab05a2d23",to:"0x587e57a516730381958f86703b1f8e970ff445d9",value:web3.toWei(3,"ether")})
</pre>

![](https://i.imgur.com/kPjjNO1.png)

这个是以太坊的一个保护机制，每隔一段时间账户就会自动锁定，这个时候任何以太币在账户之间的转换都会被拒绝，除非把该账户解锁.

### 解锁 ###
<pre>
personal.unlockAccount("0x1e173732f1d194c95f9293f120a9b6bab05a2d23")
</pre>

![](https://i.imgur.com/uCyc3DZ.png)

解锁成功

继续转账

![](https://i.imgur.com/mtrHZpW.png)

余额不足，嘿嘿。

到此你可以基本了解以太坊了，深入网上查看其它材料。

