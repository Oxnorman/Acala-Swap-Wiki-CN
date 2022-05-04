# 构建激励计划

## 1. 在提案中构建激励计划

作为上所提案的一部分，你应制定激励计划。请按照一般性[上所指南](./)操作

了解更多激励计划，请点击[这里](../../gai-shu/ji-li-ji-hua.md)

## 2. 在激励池中存入奖励

激励池是一个由治理管控的账户，该账户没有私钥。一旦你将资金转入到池子中，你就可以通过激励计划来提出一个提案，进而分发池中奖励。对于没有使用的资金，你也可以提出提案将他们转走。

* **Acala上的激励池：**`23M5ttkmR6KcoUwA7NqBjLuMJFWCvobsD9Zy95MgaAECEhit`
* **Karura上的激励池：**`qmmNufxeWaAVN8EJK58yYNW1HDcpSLpqGThui55eT3Dfr1a`

请在存入资金钱联系我们，以确保地址的准确性。

## 3. 提交链上提案

一旦准备好进行投票了，你就可以按照如下的步骤来创建一个链上提案。

一个提案申请者必须满足以下条件：

* 在Acala网络上至少有200ACA，或者由理事会成员代替你递交
* 在Karura网络上至少有100KAR，或者由理事会成员代替你递交

### 1) 递交preimage

这是生成以及递交真实的链上执行程序（也就是preimage）

前往[Polkadot Webapp](https://polkadot.js.org/apps/#/democracy)，选择你想提案的网络，比如Acala或者Karura，然后导航到`Governance` - `Democracy`

![](<../../.gitbook/assets/2 (2).png>)

如下是preimage结构的总结：

* **A) 创建一个overarching batch call**
  * **B) Batch Tx #1:** 一个调用程序为激励计划设置开始时间
    * **为计划内容创建 一个batch call**&#x20;
      * **C) Batch Tx #1.1:** 设置忠诚奖励比率
      * **D) Batch Tx #1.2**: 设置奖励数额
  * **E) Batch Tx #2:** 一个调用程序来为激励计划设置结束时间&#x20;
    * **为计划项目创建一个batch call**&#x20;
      * **F) Batch Tx #2.1:** 设置忠诚奖励比率为0
      * **G) Batch Tx #2.2**: 设置奖励数额为0

如下是一个手把手教你如何创建preimage的指南

**A) 创建一个overarching batch call**

点击 `Submit preimage` 按钮.

选择 `utility.batchAll`&#x20;

**B) 创建 Batch Tx #1:** 一个调用程序来为激励计划设置开始时间

* 点击 `Add item`
* **选择 `scheduler.schedule` 来让在一个特定的区块开始执行提案**
* **时间：输入让计划执行的区块高度**. 比如，如果你的Token将要上所，并且会在下周二区块高度为#365505的位置开始交易，然后你就可以安排激励计划在同一时间开启.&#x20;
* **优先级数值**: 255 (默认, 改变成其他优先级数值可能会由于网络安全原因而导致提案被拒绝)
* **调用**: 选择 `utility.batchAll` 由于将有不少交易会被包含其中来设置计划

![](<../../.gitbook/assets/1 (7).png>)

**C) Create Batch Tx #1.1:** set up Loyalty Bonus Ratio. if a user claims the incentive reward earlier, then this part of the reward will be returned to the reward pool and shared by other liquidity providers

Click `Add Item` under the `utlity.batchAll` call

Select `incentive.updateClaimRewardDeductionRates`

Click `Add Item` under the `incentive.updateClaimRewardDeductionRates` call

* **ModuleIncentivesPoolId**: select `Dex`
* **AcalaPrimitivesCurrencyCurrencyId**: select `DexShare`&#x20;
* **Token A and Token B**
  * If the two tokens are of different types, **then they must be entered in the ascending currencyType order**&#x20;
    * e.g. `Token` shall be entered as Token A, `ForeignAsset` shall be entered as Token B
    * Check the currencyType order [here](https://github.com/AcalaNetwork/Acala/blob/54db3acd409a0b787f116f20e163a3b24101ce38/primitives/src/currency.rs#L232-L242)
  * If the two tokens are of the same type, then **they must be entered in the ascending tokenId order.**&#x20;
    * `Token's` tokenId can be looked up [here](https://github.com/AcalaNetwork/Acala/blob/54db3acd409a0b787f116f20e163a3b24101ce38/primitives/src/currency.rs#L182-L207)
    * `ForeignAsset` tokenId can be looked up here
  * If you want to list ProjectToken-aUSD, then Token A shall be aUSD, Token B shall be ProjectToken
* **u128 (Loyalty Bonus Ratio)**: 18 decimal. For example 40% as 0.4, enter `400000000000000000`

![](broken-reference)

**D) Batch Tx #1.2**: set up Reward Amount that will be distributed every 5 blocks

Click `Add Item` under the `utlity.batchAll` call

Select `incentive.updateIncentiveRewards`

Click `Add Item` under the `incentive.updateIncentiveRewards` call to add Incentive target pool

* **ModuleIncentivesPoolId**: select `Dex`
* **Dex**: AcalaPrimitivesCurrencyCurrencyId: select `DexShare`&#x20;
* **Token A and Token B**
  * If the two tokens are of different types, **then they must be entered in the ascending currencyType order**&#x20;
    * e.g. `Token` shall be entered as Token A, `ForeignAsset` shall be entered as Token B
    * Check the currencyType order [here](https://github.com/AcalaNetwork/Acala/blob/54db3acd409a0b787f116f20e163a3b24101ce38/primitives/src/currency.rs#L232-L242)
  * If the two tokens are of the same type, then **they must be entered in the ascending tokenId order.**&#x20;
    * `Token's` tokenId can be looked up [here](https://github.com/AcalaNetwork/Acala/blob/54db3acd409a0b787f116f20e163a3b24101ce38/primitives/src/currency.rs#L182-L207)
    * `ForeignAsset` tokenId can be looked up here
  * If you want to list ProjectToken-aUSD, then Token A shall be aUSD, Token B shall be ProjectToken

Click `Add Item` again under the  `incentive.updateIncentiveRewards` call to specify reward token

* **AcalaPrimitivesCurrencyCurrencyId**: select `ForeignAsset` (select `Token` if reward token is native Acala token)
* `ForeignAsset`: enter ForeignAssetId for the reward token
* **u128 (Reward Amount)**: the amount of reward token to be distributed every 5 blocks. the decimal is the same as the reward token decimal.&#x20;

![](broken-reference)

**E) Batch Tx #2:** a scheduler call to **set an end time** for Incentive Program

Click the `Add item` under **the first `utility.batchAll`** **in Step A)**

* **Select `scheduler.schedule` for the proposal to execute at a specific block.**&#x20;
* **When**: **enter block number** for the program to be ended. After this block number, incentive rewards will no longer be distributed. If users claim rewards after this block, they will receive all their share of the loyalty bonus.
* **Priority**: 255 (default, changing to other priority may cause proposal to be rejected for network safety reasons)
* **Call**: select `utility.batchAll` as there will be a couple of transactions included to end the program

**F) Batch Tx #2.1:** set Loyalty Bonus Ratio to 0

Click `Add Item` under the `utlity.batchAll` call in **Step E)**

Select `incentive.updateClaimRewardDeductionRates`

Click `Add Item` under the `incentive.updateClaimRewardDeductionRates` call

* **ModuleIncentivesPoolId**: select `Dex`
* **AcalaPrimitivesCurrencyCurrencyId**: select `DexShare`&#x20;
* **Token A and Token B**
  * Put in the same tokens in the same order as **Step C)**
* **u128 (Loyalty Bonus Ratio)**: enter 0

![](broken-reference)

**G) Batch Tx #2.2**: set Reward Amount to 0

Click `Add Item` under the `utlity.batchAll` call **** in **Step E)**

Select `incentive.updateIncentiveRewards`&#x20;

Click `Add Item` under the `incentive.updateIncentiveRewards` call to add Incentive target pool

* **ModuleIncentivesPoolId**: select `Dex`
* **Dex**: AcalaPrimitivesCurrencyCurrencyId: select `DexShare`&#x20;
* **Token A and Token B**
  * If the two tokens are of different types, **then they must be entered in the ascending currencyType order**&#x20;
    * e.g. `Token` shall be entered as Token A, `ForeignAsset` shall be entered as Token B
    * Check the currencyType order [here](https://github.com/AcalaNetwork/Acala/blob/54db3acd409a0b787f116f20e163a3b24101ce38/primitives/src/currency.rs#L232-L242)
  * If the two tokens are of the same type, then **they must be entered in the ascending tokenId order.**&#x20;
    * `Token's` tokenId can be looked up [here](https://github.com/AcalaNetwork/Acala/blob/54db3acd409a0b787f116f20e163a3b24101ce38/primitives/src/currency.rs#L182-L207)
    * `ForeignAsset` tokenId can be looked up here
  * If you want to list ProjectToken-aUSD, then Token A shall be aUSD, Token B shall be ProjectToken

Click `Add Item` again under the `incentive.updateIncentiveRewards` call to specify reward token

* **AcalaPrimitivesCurrencyCurrencyId**: select `ForeignAsset` (select `Token` if reward token is native Acala token). **Same as Step D)**
* `ForeignAsset`: **Same as Step D)**
* **u128 (Reward Amount)**: enter 0

![](broken-reference)

#### **Verify the preimage**

You can copy the `encoded call data` and share it with another person to verify all the details.

The `encoded call data` from the above example is&#x20;

> 0x0302080200c193050000ff0302087805040101000103010000002876e1158d050000000000000000780304010100010301000405010000ca9a3b0000000000000000000000000200801a060000ff0302087805040101000103010000000000000000000000000000000000780304010100010301000405010000000000000000000000000000000000

Paste it into Polkadot webapp - `Developer` - `Decode` to see execution steps.

![](broken-reference)

**Copy the preimage hash** and submit the preimage.

### **2) Submit the Proposal**

On the `Governance - Democracy` page, click on the `Submit Proposal` button, and paste **the preimage hash** there.&#x20;

Click the `Submit proposal` button, and now your Referendum Proposal is online.

![](broken-reference)

### 3) Second a Proposal

At any given time, there may be multiple proposals created. Only the most seconded (voted or supported) proposal will be tabled in for the next Referenda vote. Acala and Karura follows the same process as the Polkadot governance framework with respective set of parameters, read more on Polkadot governance process [here](https://wiki.polkadot.network/docs/maintain-guides-democracy#seconding-a-proposal).

## Create All-in-One Proposal

You can also create a proposal that contains both listing and an Incentive Program.

**Note**: if the Bootstrap did not meet the liquidity requirement, it will not open on the specified block number e.g. #356660. If your incentive program is configured to start on #356660, since no LP tokens are distributed, no rewards will be distributed either.

### 1) Submit preimage

This is to generate and submit the actual on-chain execution (namely preimage).

Go to [Polkadot webapp](https://polkadot.js.org/apps/#/democracy), choose the network you are proposal to e.g. Acala or Karura, then navigate to `Governance` - `Democracy`

![](broken-reference)

The following is a summary of the preimage structure

* **A) Create an overarching batch call**
  * **B) Batch Tx #1:** a scheduler call to list token with Bootstrap Program
  * **C) Batch Tx #2:** a scheduler call to **set a starting time** for Incentive Program
    * **create a batch call for the content of the program**
      * **C1) Batch Tx #2.1:** set up Loyalty Bonus Ratio
      * **C2) Batch Tx #2.2**: set up Reward Amount
  * **D) Batch Tx #3:** a scheduler call to **set an end time** for Incentive Program
    * **create a batch call for the content of the program**
      * **D1) Batch Tx #3.1:** set Loyalty Bonus Ratio to 0
      * **D2) Batch Tx #3.2**: set Reward Amount to 0

The `encoded call data` from the above example is as follows

> 0x03020c02009a70050000ff5b060001050100070010a5d4e8070010a5d4e813000064a7b3b6e00d130000c84e676dc11b02f915000200407e050000ff0302087805040101000103010000a0db215d000000000000000000000078030401010001030100040501000010a5d4e800000000000000000000000200801a060000ff0302087805040101000103010000000000000000000000000000000000780304010100010301000405010000000000000000000000000000000000

Paste it into Polkadot webapp - `Developer` - `Decode` to see execution steps.

![](broken-reference)

### 2) Submit a Proposal

Click the `Submit proposal` button to submit the preimage call hash.
