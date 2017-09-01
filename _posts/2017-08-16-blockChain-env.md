---
layout: post
title: 以太坊环境搭建for mac
date: 2017-08-16 00:05:13.000000000 +08:00
tags: 区块链
categories: 区块链
---
申明：本文由LiqCode原创，转载请声明出处。

关键词：geth、solidity、testrpc、truffle

### pip安装
Python有两个著名的包管理工具easy_install.py和pip。在Python2.7的安装包中，easy_install.py是默认安装的，而pip需要我们手动安装，[安装介绍](https://pip.pypa.io/en/latest/installing/#id7)。

```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python get-pip.py
```

### 安装npm
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，直接从官网下载安装即可：
https://nodejs.org/en/

### 安装solc(solidity语言编译器)
solidity智能合约开发语言

```
sudo npm install -g solc
sudo npm install -g solc-cli
```

如果安装过程中出现下列警告
```
solc-cli@^0.3.0 requires a peer of solc@^0.3.5 but none was installed
```
可使用下列命令，指定solc版本安装。
```
sudo npm -g install solc@^0.3.5 solc-cli --save-dev
```


### 安装ethereum/cpp-ethereum(以太坊客户端geth)
```
brew tap ethereum/ethereum
brew install ethereum
```

### 安装testrpc（ethereumjs-testrpc）
testrpc是在本地使用内存模拟的一个以太坊环境，对于开发调试来说，更为方便快捷，当你的合约在testrpc中测试通过后，再可以部署到geth中去。

```
sudo npm install -g ethereumjs-testrpc
```
测试是否安装成功：
```
~ testrpc 
```
安装成功执行结果：

``` 
EthereumJS TestRPC v4.0.1 (ganache-core: 1.0.1)

Available Accounts
==================
(0) 0xae399cc9fabc0ebdc4719b58113e0dc8155d95df
(1) 0x548778cac6f20dece6abd1d56f6a9a3540bd6185
(2) 0xcc66249994adf341d76adc287cbc80aa90401db6
(3) 0xf662f57e25aeeb630f5caba7d9197d89feb9448f
(4) 0x4a435c6c7e4f9fe850f060e5307c1c5d8a3aef64
(5) 0xf36b4d4dbc5f478366ec771b30fa1b37ef63b0be
(6) 0xd1a66acd9c8fbdc881f8411184588502ab5f9435
(7) 0xc7fd296c974dc08f8f2946a3c6140b9e97630240
(8) 0x7e7f201c0df2fc603c8dac988fcd88a5b734c6b3
(9) 0x5d9bbb825dfdcec2802513416ee7c46a84c032fe

Private Keys
==================
(0) fb8e3c8097b91ed66c10b2083a333e1756b2dcf2f2392912ff76bb6561d18c5b
(1) d5a5fb6656187592e6246f1821e9355d43e10e16bd50e4438897c959bc61c7e5
(2) bfa8ea564255d289277e3fac5a97730c6041ae5324bbea2a0dd548b2faa825ef
(3) 63541eb2ea613418f9fa0b5eb115a9c213ab5de9686132b09e9092aa0164d839
(4) da49220186a60e94ce46372801dea3739d61227bd9d4b9758ffb4f96d69f56d4
(5) f881c308df33cd56f2e7750ab6d52ae79f2b90f4c736e71e9e04b8131278391e
(6) cee2d23fc96f06467590d57a84ffa767cb0b6309586a3d76bdbd192c5974ec6f
(7) fb98df56c1202872db91510c1e930fd9a1b01e287ffa970b206360f6e578e994
(8) f9ab0bec9e52fbb0ee4032a13b6831d14010c27429a738385d30aa9904148893
(9) a3b9ba73d0168162915f1259de31323cda88f159d6ccf38b468dce6e39517635

HD Wallet
==================
Mnemonic:      oyster jump apple arrest few style monkey vehicle bicycle prize property nothing
Base HD Path:  m/44'/60'/0'/0/{account_index}

Listening on localhost:8545
```

### 安装truffle
 truffle是本地的用来编译、部署智能合约的工具

```
npm install -g truffle
```

### Q&A

#### 问题一
在安装完geth和solc后，发现在geth控制台中不能编译solidity，以下是解决方案：
网上建议执行的命令如下：
brew update
brew upgrade
brew tap ethereum/ethereum
brew install solidity
brew linkapps solidity 

但是在执行完最后一个brew linkapps solidity后，终端中没有任何的返回，其实表示最后一行命令是执行失败的。
我们在执行完brew install solidity后终端会提示使用"brew link solidity"进行链接，而不是网上说的使用“brew linkapps solidity”. 
所以执行brew link solidity.   终端提示需要强制链接并重写，然后再执行命令如下：
brew link --overwrite solidity
执行完以上步骤后，就可以在geth中就可以找到solc编译器了：
eth.getCompilers()
或者
web3.eth.getCompilers()
就可以返回编译器名称了。



###### 参考：
[区块链-以太坊开发环境搭建介绍](http://blog.csdn.net/chenyufeng1991/article/details/53454228)
[简介truffle和testrpc](http://blog.csdn.net/wo541075754/article/details/53155578)

