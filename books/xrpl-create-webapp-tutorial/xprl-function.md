---
title: "接続したウォレットの残高をXRPLで取得する"
---

# 第 3 章 接続したウォレットの残高を XRPL で取得する

この章では、前章でのユーザー認証を活用して、XRPL ライブラリも使いながら機能実装を進めていきます。

# XRPL のインストール

XRP Ledger とやりとりするための JavaScript/TypeScript ライブラリをインストールします。

https://www.npmjs.com/package/xrpl

```
$ yarn add xprl
```

# 接続したウォレットの残高を取得する

トップページに、XRPL のパッケージをインポートします。

```typescript:src/App.tsx
import { Client, AccountInfoRequest } from 'xrpl';
```

さらに、残高を読み込む処理を追加します。
XRPL では、クライアントの指定が必要なため、ここではテストネットを指定しています。

```typescript:src/App.tsx
export default function App() {
  const [account, setAccount] = useState<string | undefined>(undefined);
  // 追加: 残高を入れる変数
  const [balance, setBalance] = useState<string | undefined>(undefined);

  useEffect(() => {
    xumm.user.account.then((account: string | undefined) => setAccount(account));
  }, []);

  // 追加: ページ読み込み時に残高を取得する処理を追加
  useEffect(() => {
    const fetchBalance = async () => {
        if (account) {
            // テストネットを指定
            const client = new Client('wss://testnet.xrpl-labs.com');
            await client.connect();

            const request: AccountInfoRequest = {
                command: 'account_info',
                account: account,
                ledger_index: 'validated'
            };

            const response = await client.request(request);
            setBalance(response.result.account_data.Balance);
            await client.disconnect();
        }
    };

    fetchBalance();
  }, [account]);

  const connect = async () => {
    await xumm.authorize();
  };
```

見た目の部分は以下のようなコードになります。

```typescript:src/App.tsx
return (
    <>
        <div className="relative w-full h-[350px]">
            <div
                className="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-lg text-stone-50">
                {!account && (
                    <div
                        className={"mt-3 btn btn-wide"}
                        onClick={connect}>
                        ウォレットを接続してはじめる
                    </div>
                )}
                {account && (
                    <div>
                        <div
                            className={"mt-3 btn btn-wide"}
                            onClick={connect}>
                            Account ID: {account}
                        </div>
                        <div>
                            {balance !== undefined ? `XRP Balance: ${Number(balance)/1000000}` : 'Fetching balance...'}
                        </div>
                    </div>
                )}
            </div>
        </div>
    </>
);
```

![](https://storage.googleapis.com/zenn-user-upload/fb52561e570b-20240619.png)

注意すべき点としては、残高は準備金も合計した値になっています。
