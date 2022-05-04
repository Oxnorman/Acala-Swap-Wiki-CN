# SDK

Swap的SDK 旨在为涉及Token Swap所有参数的计算提供一个简单的接口，参数包括：滑点，价格影响，供需Token等等。

在启动任何兑换之前，需要调用程序来检查流动性以及计算一个基本的参数来让兑换更加的安全并且避免一些恶意行为，比如抢先交易，或者三明治攻击.

首先，你需要创建一个SwapPromise或者SwapRx的示例:

```
  const swapPromise = new SwapPromise(api);
```

设置完Swap的参数之后:

```
  const acaToken = walletPromise.getToken("ACA");
  const ausdToken = walletPromise.getToken("AUSD");

  const path = [acaToken, kusdToken] as [Token, Token];
  const supplyAmount = new FixedPointNumber(1, acaToken.decimal);
  // set slippage 1 %
  const slippage = new FixedPointNumber(0.01);
```

然后将计算好的参数与下一个接口连接:

```
const parameters = swapPromise.swap(path: Token[], amount: FixedPointNumber, type: "EXACT_INPUT" | "EXACT_OUTPUT");
```

`type` 定义了Swap应当与供应一直还是与目标数据一致 .

你可以在这里找到全代码示例: [swapWithSDK.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/swapWithSDK.ts)
