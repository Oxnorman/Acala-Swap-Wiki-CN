# 创建一个新池

## 1. 提议创建一个新池

任何人都可以提议创建一个新池子，这个提议会在链上进行表决。为了争取更多的支持和参与，你可以使用如下的提议模板并且递交到论坛上来收集社区的意见和反馈。

{% content-ref url="ti-an-mo-ban.md" %}
[ti-an-mo-ban.md](ti-an-mo-ban.md)
{% endcontent-ref %}

* 创建一个新的池子
* 创建一个具有Bootstrap计划的池子
* 添加激励计划
* 一站式提案

## 2. 递交链上提案

一旦准备好进行投票了，你就可以按照如下的步骤来创建一个链上提案。

一个提案申请者必须满足以下条件：

* 在Acala网络上至少有200ACA，或者由理事会成员代替你递交
* 在Karura网络上至少有100KAR，或者由理事会成员代替你递交

### 提案 - 创建一个新池

#### 1) 递交preimage

这是生成以及递交真实的链上执行程序（也就是preimage）

前往[Polkadot Webapp](https://polkadot.js.org/apps/#/democracy)，选择你想提案的网络，比如Acala或者Karura，然后导航到`Governance` - `Democracy`

![](<../../.gitbook/assets/2 (2).png>)

点击 `Submit preimage`按钮，生成提案的哈希值。

**A) 创建一个scheduler call 为上所设置一个开始时间**

* **选择`scheduler.schedule`，为提案设置开始执行的特定区块**
* **时间：输入Bootstrap 计划开始的区块链高度**
* **优先级数值：**255（默认值，如果改成其他优先级数值可能会导致提案因安全问题而被拒绝）
* **Call**：选择`dex.enableTradingPair`

**B) 输入要上所的token对**

选择Token A 以及Token B（请按照[指南](../chuang-jian-yi-ge-xin-token.md)来创建新Token）

**复制preimage的哈希值**然后递交preimage。

![](<../../.gitbook/assets/1 (5).png>)

**2）递交提案**

在`Governance` - `Democracy`页中，点击`Submit Proposal` ，然后将**preimage 的哈希值**粘贴到这里

点击`Submit Proposal`，然后你就可以看到你的公民投票提案已经在线了

![](<../../.gitbook/assets/1 (12).png>)

**3）支持提案**

在任何时候，都可能有多个提案产生。只有得到最多附议（投票或者支持）的提案才会被提交给下一次公投。Acala和Karura都遵循Polkadot的治理框架，但使用自己的一套参数。[在此](https://wiki.polkadot.network/docs/maintain-guides-democracy#seconding-a-proposal)阅读更多关于Polkadot治理的信息。

### 提案 - 创建一个具有Bootstrap计划的新池

如果你想让配备Bootstrap计划的token上所，请参考如下指南：

**1) 递交preimage**

这是生成以及递交真实的链上执行程序（也就是preimage）

前往[Polkadot Webapp](https://polkadot.js.org/apps/#/democracy)，选择你想提案的网络，比如Acala或者Karura，然后导航到`Governance` - `Democracy`

![](<../../.gitbook/assets/2 (2).png>)

点击 `Submit preimage`按钮，生成提案的哈希值。

**A) 创建一个scheduler call 为上所设置一个开始时间**

* **选择`scheduler.schedule`，为提案设置开始执行的特定区块**
* **时间：输入Bootstrap 计划开始的区块链高度**
* **优先级数值：**255（默认值，如果改成其他优先级数值可能会导致提案因安全问题而被拒绝）
* **Call**：选择`dex.listProvisioning`

![](<../../.gitbook/assets/1 (8).png>)

**B) 输入Bootstrap计划的参数**

* **Token A and Token B:** 选择资产的类型和资产的标志，或者输入tokenid
  * `Token currencyid` 是网络原生资产
  * `ForeignAssets currencyid` 值得是从其他链上的资产
* **minContributionA:** A资产的最小贡献数额，比如 1 token. 小数点与贡献的Token A的小数点保持一致&#x20;
* **minContributionB:** B资产的最小贡献数额，比如 1 token. 小数点与贡献的Token B的小数点保持一致&#x20;
* **targetProvisionA**: A 资产流动性门槛&#x20;
* **targetProvisionB**: B 资产流动性门槛&#x20;
* **Not Before**: 输入区块高度，没有达到目标启动高度，池子不会开启

注意：你必须输入完整的小数位。查询适用于[Acala 资产](https://app.gitbook.com/s/X0fjyKavAAozAGhuu7sU/ru-men/acala-wang-luo/qian-bao-zhang-hu/acala-zi-chan)和[Karura资产](https://app.gitbook.com/s/X0fjyKavAAozAGhuu7sU/ru-men/karura-wang-luo/karura-zi-chan)的小数位

一个流动性池子只有满足如下条件时才会开启：

* 达到目标区块高度
* Target Provision for A或Target Provision for B已经满足
* A和B都有>0的流动性

请记得要复制preimage哈希值以及递交preimage

#### 2）递交提案

在`Governance` - `Democracy`页中，点击`Submit Proposal` ，然后将**preimage 的哈希值**粘贴到这里

点击`Submit Proposal`，然后你就可以看到你的公民投票提案已经在线了

![](<../../.gitbook/assets/1 (9).png>)

**3）支持提案**

在任何时候，都可能有多个提案产生。只有得到最多附议（投票或者支持）的提案才会被提交给下一次公投。Acala和Karura都遵循Polkadot的治理框架，但使用自己的一套参数。[在此](https://wiki.polkadot.network/docs/maintain-guides-democracy#seconding-a-proposal)阅读更多关于Polkadot治理的信息。

#### 4) Bootstrap结束后

一旦流动性池子满足如下条件：

* 达到目标区块高度
* Target Provision for A或Target Provision for B已经满足

你需要发送如下交易以出发交易持开启交易：

导航到`Developer`-`Extrinsics` - `Submission`

选择`dex.endProvisioning`

当你创建Bootstrap计划时，Token A 和Token B将会一样。

![](<../../.gitbook/assets/1 (2).png>)

递交交易后你的交易池就可以开启交易了。
