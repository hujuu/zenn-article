---
title: "【GAS】SpreadSheetから一括メール送信"
emoji: "📨"
type: "tech"
topics: ["gas"]
published: false
---

メルマガツールを使うまでもない規模のメール一括送信をしたいときに使えそうな GoogleAppScript を作りました。
あと文面が各々微妙に異なるなどメルマガツールでは要件的に実現しづらいケースにも活用できるかと思います。

# 肝心の GAS コード

https://gist.github.com/hujuu/a520e8473cd79aa42069167f7f4bd382

```
function email() {
  var response = Browser.msgBox("メールを送信します", "送信してよいですか？", Browser.Buttons.OK_CANCEL);
  if(response === 'cancel') {
	return;
  }
  const sheet = SpreadsheetApp.getActiveSheet();
  const last_row = sheet.getLastRow();
  for(let i = 2; i <= last_row; i++) {
	if(sheet.getRange(i, 4).getValue() === 'Done'){
	  continue;
	}
	Logger.log(sheet.getRange(i, 1).getValue());
	let target = sheet.getRange(i, 1).getValue();
	let subject = sheet.getRange(i, 2).getValue();
	let body = sheet.getRange(i, 3).getValue();
	let options = {from: "Sample <support@example.com>"};
	GmailApp.sendEmail(target, subject, body, options);
	sheet.getRange(i, 4).setValue('Done');
  }
}
```

# スプレッドシートのフォーマット

A 列:宛先メールアドレス, B 列:メールタイトル, C 列:メール文面, D 列:送信結果
というフォーマットでスプレッドシートを作成します。

![フォーマット](https://storage.googleapis.com/zenn-user-upload/42e2d1e2f17c-20220601.png)

# スクリプトの作成

GoogleAppScript のエディタを開いて、上記のコードを入力します。
保存したら、先ほどのスプレッドシートに戻ります。

![](https://storage.googleapis.com/zenn-user-upload/1060dda12aad-20220601.png)

# スクリプト実行ボタンの作成

挿入 > 図形描画 からボタンオブジェクトを作成します。

![](https://storage.googleapis.com/zenn-user-upload/94eedfe419bf-20220601.png)

ボタンが作成できたら、ボタン選択時右上に表示される 3 連点をクリックして「スクリプトを割り当て」を選択します。
表示されるダイアログに、先ほど作成した GAS の関数名を入れます。
上記のコードをそのまま使う場合は、 `email` と入れます。

![](https://storage.googleapis.com/zenn-user-upload/6165f161016a-20220601.png)

## ボタンをクリックしたら、メール送信実行！

実行が成功した行には、D 列に Done と入ります。
もし失敗があっても再実行すると、Done が入っていない行だけが実行されます。

# 注意点：メール送信元

メール送信元として使えるメールアドレスは、
GAS を実行する Google アカウントのメールアドレスまたはオーナー権限を持っている GoogleGroup のメールアドレスです。
