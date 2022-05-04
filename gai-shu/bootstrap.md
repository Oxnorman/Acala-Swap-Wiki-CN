# Bootstrap

## 什么是Bootstrap?&#x20;

项目方可以使用Bootstrap来启动他们的Token计划：

* 项目方可以配置Bootstrap，使得资产对在完全自由交易之前，符合条件的用户可以向池子添加流动性&#x20;
* 项目方可以将项目Token存入其中&#x20;
* 交易者可以存入项目Token、交易Token（如aUSD）或两者同时参与。&#x20;
* 一旦流动性目标得到满足，Bootstrap结束，流动性池将开启交易&#x20;

这使得价格在整个Bootstrap期间得到巩固和稳定，并阻止了“反向”操作。比如，Bootstrap可以阻止大户和交易机器人一次性全部买入并进行超前交易。&#x20;

Bootstrap结束时的项目Token价格会成为开盘交易价格。&#x20;

## Bootstrap持续多长时间？&#x20;

Bootstrap的持续时间由发起人决定。&#x20;

## 我应该在Bootstrap启动后立即加入吗？&#x20;

不需要，价格后期预计会逐渐稳定下来。&#x20;

## 我该用一种还是两种Token加入？&#x20;

这取决于你自己。

* **只用项目Token（或A Token）加入时**：当资金池开放时，你将在市场上出售A Token。&#x20;
* **使用交易Token加入，例如aUSD（或B Token）：**当资金池开放时，你将在市场上购买项目Token（或A Token）。&#x20;
* **以两种Token加入：**你可以投入你认为公平的价格，例如，如果你认为A Token价值2美元，你可以放入10个A Token以及20个aUSD
  * 如果开盘价格较高：你的A Token将会部分卖出以弥补同等价值的LP。A Token将以比你预期高的价格卖出
  * 如果开盘价格较低：部分A Token将被买入，以弥足等值的LP，你以低于你预期的价格买入更多的A Token。&#x20;
  * 如果开盘价相同：你的持有量没有变化

## 模拟：Bootstrap期间&#x20;

在Bootstrap期间：

* **不允许交易**，因此可以合并汇率&#x20;
* 你可以同时为A Token，或B Token，或A和B Token贡献流动性。A和B Token的实际汇率只有在Bootstrap完成后才能知道。&#x20;
* 在你出资时，你会被分配到一定数量的LP代币，以及池子里的指示性LP份额，这些份额会随着更多流动性的增加而改变。&#x20;
* 只有你想成为资金池的流动性提供商时你才应该参与。请注意成为流动性提供商的相关各种风险。&#x20;

## LP Token和LP份额

LP Token是贡献流动性到资金池的收据。LP份额（=LP代币/LP代币总量）是对某一特定池子的流动性贡献的比例表示，例如A Token-B Token池。&#x20;

以下公式用于计算对A Token-B Token池贡献后的LP Token：

$$
LP Tokens = Token A Contribution * 1 + Token B Contribution * Exchange Rate
$$

下面是模拟在Bootstrap期间，当更多的流动性被注入到池子里时，用户在池子里的LP份额将会如何变化：

* 用户1只贡献了1000个B Token。LP代币只有在池子的两边都有一定流动资金后才能计算出来。&#x20;
* 在用户2注入他/她的流动性资金后（用户2只贡献了50个A Token），
  * A Token-B Token的汇率为0.05（=50/1000，基于[恒定乘积](liu-dong-chi.md)），
  * 用户2的LP Token是50（=50_1 + 0 \* 0.05）_
  * 用户1的LP Token也是50 (=10 + 1000 \* 0.05)&#x20;
  * 用户2的LP份额是50% (=50/(50+50))&#x20;
  * 用户1的LP份额也是50%。&#x20;
* 在用户4加入他/她的流动性资金后（用户4同时贡献了A和B Token）&#x20;
  * 代币A-代币B的汇率为2 (=2200/1100)&#x20;
  * 用户4的LP份额是52.27%
  * 用户2的LP份额下降到1.14%
  * 用户1的LP份额下降到45.45%
*   如果Bootstrap在该时间点结束

    * 用户1可以赎回1000个A Token（=220045.45%）和500个B Token（=110045.45%）。&#x20;
    * 用户2可以赎回25个A Token和12.5个B Token&#x20;
    * 用户4可以赎回1150个A Token和575个B Token
    * LPs可能会也可能不会在Bootstrap之后马上赎回，是因为大部分回报来自于交易费用。在[这里](bootstrap.md)阅读更多信息。&#x20;



因此，如果你只贡献交易池子的一种Token（比如说A Token），你实际上是根据Bootstrap结束（即池子开放交易）时的汇率将50%的A Token转换成B Token。

![在Bootstrap期间，LP 份额的变动](<../.gitbook/assets/1 (13).png>)

![LP Token的计算](<../.gitbook/assets/2 (1).png>)

模拟：Boostrap结束之后

让我们假设资金池的Bootstrap在第4个用户的贡献后结束（如上图所示），然后LP Token被分配给每个流动性提供者。LP份额（=LP Token/LP Token总数）是LP对特定池子的总体流动性的贡献以比例表示。然后，LP Token可以在任何时候被赎回为底层资产（A和B Token）。比如，用户1可以赎回45.45%的A Token（1000个A Token=2200\*45.45%）和45.45%的B Token（500个B Token=1100\*45.45%）。&#x20;

* 你需要在Bootstrap完成后认领你的LP Token
* A Token-B Token池将被启用进行交易&#x20;

下面是的例子将阐释这一点：

![](<../.gitbook/assets/3 (1).png>)

### 领取LP Token

Bootstrap结束，LP Token就会分配给LP，并可通过以下方式领取&#x20;

* [AcalaSwap Web Application](https://app.gitbook.com/s/I60OMjSmrBgy1AoE1I84/) 或&#x20;
* 在Polkadot Web App上直接调用`dex.claimDexShare` - 在适当的网络上
