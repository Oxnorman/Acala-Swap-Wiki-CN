# 创建一个新Token

总体上有四种不同类型的新Token

1. **从Statemine/Statemint的桥接资产**
2. **从其他平行链的桥接资产**
3. **在EVM+上创建ERC20 Token**
4. **创建原生资产**

## 0. 创建链下提案

查阅指南，[点击这里](chuang-jian-yi-ge-xin-chi/ti-an-mo-ban.md)

## 1. **从Statemine/Statemint的桥接**

[Statemint](https://polkadot.network/blog/statemine-upgrade-launches-new-phase-of-parachain-functionality/)是波卡的公共利益平行链，Statemine是[Kusama](https://polkadot.network/blog/statemine-upgrade-launches-new-phase-of-parachain-functionality/)的公共利益平行链。

* 你可以桥接Statemine资产（在Kusama上）到Karura网络中，在AcalaSwap上所
* 一旦Statemint启动，你也可以将资产桥接到Acala网络

如下的示例是通过创建一个`ForeignAsset`,将Statemine资产桥接到Karura网络上:

### A. 递交Preimage

这是生成以及递交真实的链上执行程序（也就是preimage）

前往[Polkadot Webapp](https://polkadot.js.org/apps/#/democracy)，选择你想提案的网络，比如Acala或者Karura，然后导航到`Governance` - `Democracy`

* 点击 `Submit preimage`按钮，然后填好相关信息
* **复制preimage的哈希值**
* 点击`submit`按钮来完成手续

![](<../.gitbook/assets/1 (6).png>)

### B. 递交提案

在`Governance` - `Democracy`页中，点击`Submit Proposal` ，然后将**preimage 的哈希值**粘贴到这里

点击`Submit Proposal`，然后你就可以看到你的公民投票提案已经在线了

## 2. 从其他平行链桥接资产

### 先决条件

在发起方平行链和目的地平行链（比如Acala或者Karura）之间的HRMP信道必须开启才能进行自查你的桥接。

请查阅配置本地测试网，集成Acala/Karura Token，开启HRMP信道的集成指南，请点击[这里](https://app.gitbook.com/s/X0fjyKavAAozAGhuu7sU/jian-she-acala/jian-she-dapps/ke-zu-he-de-lian)

### A. 递交Preimage

这是生成以及递交真实的链上执行程序（也就是preimage）

前往[Polkadot Webapp](https://polkadot.js.org/apps/#/democracy)，选择你想提案的网络，比如Acala或者Karura，然后导航到`Governance` - `Democracy`

![](<../.gitbook/assets/2 (3).png>)

* 点击 `Submit preimage`按钮，`生成提案的哈希值`
* 选择`assetRegistry.registerForeignAsset`

![](<../.gitbook/assets/1 (10).png>)

**复制preimage的哈希值**然后递交preimage。

### B. 递交提案

在`Governance` - `Democracy`页中，点击`Submit Proposal` ，然后将**preimage 的哈希值**粘贴到这里

点击`Submit Proposal`，然后你就可以看到你的公民投票提案已经在线了

## 3. 在EVM+上创建ERC20 token

即将推出

## 4. 创建原生资产

如下是preimage的一个示例。先递交一个preimage然后递交公投提案。

![](<../.gitbook/assets/1 (4).png>)
