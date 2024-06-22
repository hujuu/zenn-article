---
title: "XRPの送金機能を実装する"
---

# 第 4 章 XRP を送金する

この章では、接続したウォレットから、XRP を指定のウォレットに送信する機能を実装します。

# 送金をおこなうコンポーネントを作成する

送金をおこなう機能をもったコンポーネントを作成していきます。
コンポーネントをまとめておく、`components`フォルダを作成します。

```
$ mkdir components
```

コンポーネントのファイルは以下のようになります。

```typescript:components/PaymentForm.tsx
import { xumm } from "../store/XummStore";

export default function PaymentForm({ account }: { account: string | undefined }) {
    const createTransaction = async () => {
        const payload = await xumm.payload?.create({
            TransactionType: "Payment",
            Destination: "r4aNu6fs5HuS6vBrHrTbNQhp2QbsX9qPSw",
            Amount: "1000", // 1000 drops (=0.001000XRP)
        });
        if (!payload?.pushed) {
            // XummへPush通知が届かない場合の処理
        }
    };

    return (
        <div>
            {account && (
                <div>
                    <div className="relative mb-4">
                        <div className="absolute inset-0 flex items-center" aria-hidden="true">
                            <div className="w-full border-t border-gray-300"/>
                        </div>
                        <div className="relative flex justify-start">
                            <span className="bg-white pr-3 text-base font-semibold leading-6 text-gray-900">XRPの送金</span>
                        </div>
                    </div>
                    <button
                        type="button"
                        className="rounded-md bg-white px-2.5 py-1.5 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
                        onClick={createTransaction}>Payment
                    </button>
                </div>
            )}
        </div>
    );
}
```

送金を実行している処理を少し見ていきましょう。

```typescript:src/App.tsx
const createTransaction = async () => {
    const payload = await xumm.payload?.create({
        TransactionType: "Payment",
        Destination: "r4aNu6fs5HuS6vBrHrTbNQhp2QbsX9qPSw",
        Amount: "1000", // 1000 drops (=0.001000XRP)
    });
    if (!payload?.pushed) {
        // XummへPush通知が届かない場合の処理
    }
};
```

`TransactionType`を`Payment`に指定しています。
`Destination`は送金先のアドレスを入れます。ここでは、テスト用のアドレスを入れていますが、ご自分でもう一つのアカウントを作成して、そのアドレスを指定して送金することも可能です。
`Amount`は送金する XRP の量を指定します。量は drop という単位で指定します。drop は、XRP の最小単位である 1XRP の 100 万分の 1 となっています。なので、1000 を指定すれば、0.001000XRP 送金されるようになります。

# コンポーネントを読み込み、送金を試す

それでは、作成したコンポーネントをトップページで読み込んで表示してみましょう。

```typescript:src/App.tsx
import { useEffect, useState } from 'react';
import { xumm } from "./store/XummStore";
import { Client, AccountInfoRequest } from 'xrpl';
// 追加: 送金コンポーネント
import PaymentForm from "./components/PaymentForm.tsx";

export default function App() {
    ・・・・・・(省略)・・・・・・

    return (
        <div className="relative w-full h-[350px]">
            <div
                className="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-lg text-stone-50">
                {!account && (
                    <div className={"h-1/2"}>
                        <div
                            className="rounded-md bg-white px-2.5 py-1.5 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
                            onClick={connect}>
                            ウォレットを接続してはじめる
                        </div>
                    </div>
                )}
                {account && (
                    <div>
                        <div className={"mt-3 text-gray-800"}>
                            Account ID: {account}
                        </div>
                        <div className={"mt-3 mb-8 text-gray-800"}>
                            {balance !== undefined ? `XRP Balance: ${Number(balance) / 1000000}` : 'Fetching balance...'}
                        </div>
                        {/* 追加：送金コンポーネントの追加 */}
                        <PaymentForm account={account}/>
                    </div>
                )}
            </div>
        </div>
    );
}
```

サービスを立ち上げると画像のように、ボタンを追加されます。

![](https://storage.googleapis.com/zenn-user-upload/b2bcce5b6f38-20240622.png)

ボタンをクリックすると、Xaman へ送金確認の通知が届きます。
送金確認を承認すると、送金が完了します。

もし、サンプルコードのアドレスに送金した場合は、以下のサイトから送金が成功しているか確認することが可能です。

https://test.bithomp.com/explorer/r4aNu6fs5HuS6vBrHrTbNQhp2QbsX9qPSw

![](https://storage.googleapis.com/zenn-user-upload/abc187efcf9c-20240622.png)

# 送金先や送金額をブラウザから操作できるようにする

ここまでで、送金をできるようになりましたが、送金先や送金額もコードで指定された値に固定されています。
次のステップとしてブラウザのフォームで、送金先や送金額を編集できるようにしてみましょう。
送金先と送金額の入力フォームを作成して、その値を送金処理に反映します。

```typescript:components/PaymentForm.tsx
import { xumm } from "../store/XummStore";
// 追加
import {useState} from "react";

export default function PaymentForm({ account }: { account: string | undefined }) {
    // 追加: アドレスを格納する変数
    const [destinationAddress, setDestinationAddress] = useState('');
    // 追加: 送金額を格納する変数
    const [transactionAmount, setTransactionAmount] = useState('');

    const createTransaction = async () => {
        const payload = await xumm.payload?.create({
            TransactionType: "Payment",
            // 変更: フォームに記入されたアドレス指定
            Destination: destinationAddress,
            // 変更: フォームに記入された送金額を指定
            Amount: String(Number(transactionAmount)*1000000),
        });
        if (!payload?.pushed) {
            // XummへPush通知が届かない場合の処理
        }
    };

    return (
        <div>
            {account && (
                <div>
                    <div className="relative mb-4">
                        <div className="absolute inset-0 flex items-center" aria-hidden="true">
                            <div className="w-full border-t border-gray-300"/>
                        </div>
                        <div className="relative flex justify-start">
                                    <span
                                        className="bg-white pr-3 text-base font-semibold leading-6 text-gray-900">XRPの送金</span>
                        </div>
                    </div>
                    {/* 追加：送金先フォーム */}
                    <div className="relative mb-4">
                        <label
                            htmlFor="name"
                            className="absolute -top-2 left-2 inline-block bg-white px-1 text-xs font-medium text-gray-900"
                        >
                            送金先アドレス
                        </label>
                        <input
                            type="text"
                            name="wallet-address"
                            id="wallet-address"
                            className="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                            placeholder=""
                            onChange={(e) => setDestinationAddress(e.target.value)}
                        />
                    </div>
                    {/* 追加：送金額フォーム */}
                    <div className="relative mb-4">
                        <label
                            htmlFor="name"
                            className="absolute -top-2 left-2 inline-block bg-white px-1 text-xs font-medium text-gray-900"
                        >
                            送金額
                        </label>
                        <input
                            type="text"
                            name="amount"
                            id="amount"
                            className="block w-1/2 rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                            placeholder="1000"
                            onChange={(e) => setTransactionAmount(e.target.value)}
                        />
                        <div className="pointer-events-none absolute inset-y-0 right-1/2 flex items-center pr-3">
                                  <span className="text-gray-500 sm:text-sm" id="price-currency">
                                    XRP
                                  </span>
                        </div>
                    </div>
                    <button
                        type="button"
                        className="rounded-md bg-white px-2.5 py-1.5 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
                        onClick={createTransaction}>Payment
                    </button>
                </div>
            )}
        </div>
    );
}
```

見た目はこちらのようになります。

![](https://storage.googleapis.com/zenn-user-upload/96f6e03a3d08-20240623.png)

送金額の指定について補足していきます。
1 点目は、UI 上は XRP で指定するような見た目にしましたので、入力された値の 1000000 倍しています。
2 点目に、Amount は数字を指定しますが、リクエストとして**文字列**で送信する必要がありますので、計算したあとに、文字列に戻しています。

```typescript:components/PaymentForm.tsx
const createTransaction = async () => {
    const payload = await xumm.payload?.create({
        TransactionType: "Payment",
        // 変更: フォームに記入されたアドレス指定
        Destination: destinationAddress,
        // 変更: フォームに記入された送金額を指定
        Amount: String(Number(transactionAmount)*1000000),
    });
    if (!payload?.pushed) {
        // XummへPush通知が届かない場合の処理
    }
};
```

これで、好きな相手に好きな額の XRP を送れる機能を作れるようになりました。
