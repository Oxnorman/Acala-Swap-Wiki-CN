# API

请注意：如下示例代码使用Acala 网络资产作为例子

## 类型

我们将列举在Typescript中的类型以及相应代表.

### CurrencyId

```javascript
type CurrencyId {
    TOKEN: string
}
```

_示例_:

> const acaCurrencyId = { TOKEN: ‘ACA’ }

### 交易对

```javascript
type TradingPair = [CurrencyId, CurrencyId]
```

_示例_:

> const tradingPair = \[{ TOKEN: ‘ACA’ }, { TOKEN: ‘AUSD’ }];

### 交易对状态

```javascript
type TradingPairStatus = {
    Enabled?: null,
    NotEnabled?: null,
}
```

I显示交易对是否被启用:

* `{ Enabled: null }` - 已启用
* `{ NotEnabled: null }` - 还未启用

## 用Acala wrapper初始化 Polkadot.js 的API Provider

你可以使用 `@acala-network/api` 来获取可用方法的元数据描述。\
设置 polkadot/api provider 示例：

```javascript
    const provider = new WsProvider('<NODE_WS_ADDRESS>');
    const api = new ApiPromise( options({ provider }) );
    await api.isReady;
    ...
```

你可以查看可用的[公共端点列表](https://app.gitbook.com/s/X0fjyKavAAozAGhuu7sU/ji-cheng-dao-acala/karura/karura-wang-luo-xin-xi)

**全代码摘要:**

[dex-examples/getPolkadotApi.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/utils/getPolkadotApi.ts)

## 只读功能（状态查询）

这些功能只能读取链上信息，因此不需要用私钥签署交易。阅读更多状态查询信息，请点击[状态查询文件](https://polkadot.js.org/docs/api/start/api.query/)。&#x20;

### 获取系统参数

相关系统参数如token可用位数，符号，风险参数和更多可以链上读取的信息.

```javascript
const properties = await api.rpc.system.properties();
const decimals = !result.tokenDecimals.isNone && 
    result.tokenDecimals.value.toHuman();
const symbols = !result.tokenSymbol.isNone && 
    result.tokenSymbol.value.toHuman();
...
```

**全代码摘要:**

[dex-examples/getSystemParameters.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/utils/getSystemParameters.ts)

### 获取流动性

将`Token A`和`Token B`交易对的流动性返还到池子中.

```javascript
liquidityPool(tradingPair: TradingPair): 
    [Balance, Balance]
```

> 阅读更多 `Balance` 类型，请点击[这里](https://polkadot.js.org/docs/api/start/typescript/#storage-generics)。

参数：

| 名称          | 类型          | 内容                                                              |
| ----------- | ----------- | --------------------------------------------------------------- |
| tradingPair | TradingPair | the tuple of CurrencyIds included in the desired liquidity pool |

> 注意 ![:warning:](https://assets.hackmd.io/build/emojify.js/dist/images/basic/warning.png): CurrencyIds的顺序在交易对中很重要，如果错误，交易对两边的Token余额将返回为0.

示例:

```javascript
    const liquidity = await api.query.dex.liquidityPool([
        { TOKEN: 'AUSD' },
        { TOKEN: 'ACA' },
    ]);
    console.log(liquidity.map(t => t.toHuman()))
```

**全节点摘要:**

[dex-examples/getLiquidity.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getLiquidity.ts)

### 获取Provisioning 池余额(getProvisioningPoolBalance)

Provisioning Pool 是Boostrap期间的流动性池子,也就是在交易开启之前。你可以在这里阅读更多关于[Bootstrap](../gai-shu/bootstrap.md)的信息.

getProvisioningPoolBalance 将返回在交易对中每一个Token的余额，该余额取决于该账户持有的LP token的数量。

```javascript
provisioningPool(tradingPair: TradingPair, accountId: string):
[Balance, Balance];
```

| 名称          | 类型          | 内容                                |
| ----------- | ----------- | --------------------------------- |
| tradingPair | TradingPair | currencyIds 在provisioning pair的数值 |
| accountId   | string      | 账户数据                              |

示例:

```javascript
    const liquidity = await api.query.dex.provisioningPool([
            { TOKEN: 'ACA' },
            { TOKEN: 'AUSD' }
        ], 
        't98jaBc3cdvZuQpBoiXpJW1uGsFhf9Gq6YDW4UmMtxdxZVL',
    );
```

**全代码摘要:**

[dex-examples/getProvisioningLiquidity.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getProvisioningLiquidity.ts)

### 交易对状态信息（tradingPairStatuses）

返回一个数值来显示一个交易是否开启. 请注意交易对中Token的顺序很重要

```javascript
tradingPairStatuses(TradingPair): TradingPairStatus
```

参数

| 名称          | 类型          | 内容                    |
| ----------- | ----------- | --------------------- |
| tradingPair | TradingPair | currencyIds 在预期交易对的数值 |

示例:

```javascript
const status = await api.query.dex.tradingPairStatuses([
        { TOKEN: 'ACA' },
        { TOKEN: 'AUSD' }
    ]);
```

**全代码摘要:**

[dex-examples/getTradingPairStatuses.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getTradingPairStatuses.ts)

## 状态变换功能

这些交易将数据写入链上，以及要求私钥来签名交易.\
签署者可以有不同的方法来签署交易，你可以在这里查询 [polkadot api docs](https://polkadot.js.org/docs/api).

如下是一个使用种子助记词来形成一个用户的示例，该种子助记词由Polkadot形成:

```javascript
const keyring = new Keyring({
        type: 'sr25519'
    });
const signer = keyring.addFromMnemonic('<YOUR_SEED_PHRASE>');
```

**全代码摘要:**

[dex-examples/getSigner.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/utils/getSigner.ts)

### swapWithExactSupply

```javascript
swapWithExactSupply(path: CurrencyId[], supply_amount: number, min_target_amount: number): Extrinsic
```

兑换一定数量的token A `supply_amount` 成一个最小数量的 token B\
`min_target_amount`.

交易的滑点 = `supply_amount / min_target_amount`

流动性池子当前的汇率 = `token_A_liquidity / token_B_liquidity`.

返回 `Extrinsic` 类型，该值需私钥签名.

参数

| 名称                  | 类型            | 内容                                                                              |
| ------------------- | ------------- | ------------------------------------------------------------------------------- |
| path                | CurrencyId\[] | CurrencyIds的数组. 路径数组中的第一个货币将被换成最后一个货币。如果你想交换的代币没有一个直接的池子，交易路径应该包含一个中间代币，来实现交易等等 |
| supply\_amount      | number        | 如果兑换成功，将收取确切的供应量                                                                |
| min\_target\_amount | number        | 如果兑换成功，收到的最小数量。改变该数值能改变滑点。                                                      |

示例

```javascript
    const path = [
        { TOKEN: "ACA", },
        { TOKEN: "AUSD", },
    ]
    // minTargetAmount is "0x0" which means that slippage is 100%
    const minTargetAmount = "0x0";
    const supplyAmount = <AMOUNT_DENORMALISED>;

    const extrinsic = api.tx.dex.swapWithExactSupply(
        path,
        supplyAmount,
        minTargetAmount
    );
    const hash = await extrinsic.signAndSend(signer);
    console.log('hash', hash.toHuman());
```

> 注意： ![:warning:](https://assets.hackmd.io/build/emojify.js/dist/images/basic/warning.png) 该示例中，供应的数量的小数位应与ACA的小数位一致

**全代码摘要:**

[dex-examples/swapWithExactSupply.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/swapWithExactSupply.ts)

### swapWithExactTarget

```
swapWithExactTarget(path: CurrencyId[], target_amount: number, max_supply_amount: number): Extrinsic
```

以一定数量的Token A兑换一定数量的Token B。如果池子当前的比率不需要更多而供应Token，交易就会停止.

交易的滑点定义如下:`max_supply_amount / target_amount`

流动性池子的当前比率 = `token_A_liquidity / token_B_liquidity`

返回 `Extrinsic` 的类型，该返回值由私钥签署。

参数

| 名称                  | 类型            | 内容                                                                              |
| ------------------- | ------------- | ------------------------------------------------------------------------------- |
| path                | CurrencyId\[] | CurrencyIds的数组. 路径数组中的第一个货币将被换成最后一个货币。如果你想交换的代币没有一个直接的池子，交易路径应该包含一个中间代币，来实现交易等等 |
| target\_amount      | number        | 如果兑换成功，将收取确切的供应量得到目标数量的                                                         |
| max\_supply\_amount | number        | 得到目标数量的最大花费. 更改此数值会修改滑点                                                         |

示例:

```javascript
    const targetAmount = <DENORMALISED_EXACT_AMOUNT>;

    const path = [
        { TOKEN: "ACA", },
        { TOKEN: "AUSD", },
    ]
    const maxSupplyAmount = <DENORMALISED_MAX_AMOUNT>;

    const extrinsic = api.tx.dex.swapWithExactTarget(
        path,
        targetAmount,
        maxSupplyAmount,
    );
    const hash = await extrinsic.signAndSend(signer);
```

**全代码摘要:**

[dex-examples/swapWithExactTarget.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/swapWithExactTarget.ts)

### addLiquidity

```javascript
addLiquidity(
    currency_id_a: CurrencyId, 
    currency_id_b: CurrencyId, 
    max_amount_a: number, 
    max_amount_b: number, 
    min_share_increment: number, 
    stake_increment_share: boolean
): Extrinsic
```

为交易对的池子添加流动性 `currency_id_a` and `currency_id_b`. 只要当token对启用该操作才会成功.

你可以设置A和B最大的数量 `max_amount_a` 和 `max_amount_b.` 如果你提供的他们比率与当前池子比率不一致，其中一个token将被作为最大量，另外一个将以当前池子比率计算得出.

`min_share_increment` 定义了接受到的LP Token的最小值. 这在某种程度上出于保护的目的，给潜在的漏洞设置了一个上限，比如前端运行的程序.

返回需要用私钥签名的 `Extrinsic` 类型。

> 比如，当前的池子比率为0.5，这就意味着池子中Token A的比Token B少了一倍。如果你提供了 100 `max_amount_a` 和 100 `max_amount_b`, 考虑到当前的池子比率，实际上你添加的流动性是100个Token B 以及只有 50 Token A .

参数

| 名称                      | 类型         | 内容                       |
| ----------------------- | ---------- | ------------------------ |
| currency\_id\_a         | CurrencyId | 交易对中首先接受流动性的货币           |
| currency\_id\_b         | CurrencyId | 交易对中第二个接受流动性货币           |
| max\_amount\_a          | number     | 交易对中Token A的非规范化的最大数量    |
| max\_amount\_b          | number     | 交易对中Token B的非规范化的最大数量    |
| min\_share\_increment   | number     | 你将收到额最小LP Token的最小数量     |
| stake\_increment\_share | boolean    | 如果返回值为 `true` 它将自动质押所有份额 |

**示例：**

```javascript
    const currency_id_a = { TOKEN: 'ACA' };
    const currency_id_b = { TOKEN: 'AUSD' };
    const max_amount_a = 2 * 10 ** symbolsDecimals["ACA"];
    const max_amount_b = 2 * 10 ** symbolsDecimals["AUSD"];
    // slippage is 100% for receiving shares
    const min_share_increment = 0;
    const stake_increment_share = true

    const extrinsic = api.tx.dex.addLiquidity(
        currency_id_a,
        currency_id_b,
        max_amount_a,
        max_amount_b,
        min_share_increment,
        stake_increment_share,
    );
    const hash = await extrinsic.signAndSend(signer);
```

**全代码摘要：**\
[dex-examples/addLiquidity.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/addLiquidity.ts)

> ![:warning:](https://assets.hackmd.io/build/emojify.js/dist/images/basic/warning.png) 请确保交易对中的任何一个Token都有余额以及这个交易对是可用的

### removeLiquidity

```javascript
removeLiquidity(
    currency_id_a: CurrencyId, 
    currency_id_b: CurrencyId, 
    remove_share: number, 
    min_withdrawn_a: number, 
    min_withdrawn_b: number, 
    by_unstake: boolean
): Extrinsic
```

从所选的交易对中移除流动性. 它也可以自动解除所有LP Token的质押并且取回他们。这个程序的调用整合/打包两个交易成一个交易并且只需要一次费用.

`min_withdrawn_` 定义了Token A你可以接收到的最小量.

参数

| 名称                | 类型         | 内容                                                                              |
| ----------------- | ---------- | ------------------------------------------------------------------------------- |
| currency\_id\_a   | CurrencyId | 交易对中首先接受流动性的货币                                                                  |
| currency\_id\_b   | CurrencyId | 交易对中第二个接受流动性的货币                                                                 |
| remove\_share     | number     | 移除流动性所占的份额                                                                      |
| min\_withdrawn\_a | number     | 基于你想获得的份额，交易对中Token A的最小数量。 如果当前比率不满足，交易失败                                      |
| min\_withdrawn\_B | number     | 基于你想获得的份额，交易对中Token B的最小数量。 如果当前比率不满足，交易失败                                      |
| by\_unstake       | boolean    | 如果返回值为 `true` 他将自动 会自动解绑一定量的质押LP  Token并可以取回. 如果返回值 为 `false` 它将只提取非质押的LP Token |

**示例**

```javascript
    const currency_id_a = { TOKEN: 'ACA' };
    const currency_id_b = { TOKEN: 'DOT' };
    // amount of shares to remove
    const remove_share = 1;
    // slippage for ACA is 100%, we want to receive anything that liquidity pool ratio will offer
    const min_withdrawn_a = 0;
    // slippage for DOT is 100%, we want to receive anything that liquidity pool ratio will offer
    const min_withdrawn_b = 0;
    const by_unstake = true;

    const extrinsic = api.tx.dex.removeLiquidity(
        currency_id_a,
        currency_id_b,
        remove_share,
        min_withdrawn_a,
        min_withdrawn_b,
        by_unstake
    );
```

**全节点摘要**\
[dex-examples/removeLiquidity.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/removeLiquidity.ts)
