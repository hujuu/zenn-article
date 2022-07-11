---
title: "【GAS】SpreadSheetで一括QRコード生成・保存"
emoji: "🏁"
type: "tech"
topics: ["GAS", "QRcode", "SpreadSheet"]
published: False
---

QR コードを数十個単位で一括発行・保存しようと思ったときに無料のツールが見つからなかったのですが、
SpreadSheet と GAS, Google API で生成・保存するがありましたので、まとめます。

有料であれば、すぐに使えるツールがあるので、記載しておきます。
（もしくは、分割して少しずつ生成・保存する余裕がある方も以下のツールで充分かもしれません。）

# お金を払える方向け

有料であれば、以下のツールで QR コード一括生成・保存が可能です。
いずれも無料枠は 20 個までです。

**QR コード一括作成くん**
https://qr-sakusei.com/

**QR Code Generator**
https://www.anymerge.com/qr-code-generator-for-google-spreadsheets

---

# 作成手順

## スプレッドシートのフォーマット

![スプレッドシート](https://storage.googleapis.com/zenn-user-upload/7d2a04427373-20220712.png)

A 行には、QR コード画像が生成される URL を記載します。

`="http://chart.apis.google.com/chart?chs=" & C1 & "&cht=qr&chl=" & B1`

**URL パラメータ**

- chs : 解像度 (C 列)
- chl : コードにしたい文字列 (B 列)

D 列は任意で、生成される QR コード画像のファイル名にしたい値を入れます。

## GAS の作成

https://gist.github.com/hujuu/ae5fa8b4116bceadcbbb5ef8b7b72c13

```js
/**
 * A列に入力された画像URLリストを元に画像をダウンロードしてGoogle Driveに保存
 */
function downloadImages() {
  // 作っておいた画像フォルダの情報を取得
  var folders = DriveApp.getFoldersByName("画像フォルダ");
  if (false === folders.hasNext()) {
    return;
  }
  var folder = folders.next();
  // 現在のシートを取得
  var sheet = SpreadsheetApp.getActiveSheet();
  // 読み取る範囲を指定
  var range = sheet.getRange("A1:E");
  // 画像URLが入力されている最後の行数を取得
  var row = sheet.getLastRow();

  for (i = 1; i <= row; i++) {
    // シートから1行ずつ画像URLを取得
    var url = range.getCell(i, 1).getValue();
    var token = range.getCell(i, 4).getValue();
    // 画像データを取得
    var response = UrlFetchApp.fetch(url);
    var fileBlob = response.getBlob().setName("QR_" + token);
    // 取得した画像をGoogle Driveにアップロード
    folder.createFile(fileBlob);
  }
}
```

# 実行

保存先として、Google Drive に”画像フォルダ”という名前のフォルダを作成しておきます。

Apps Script 画面から実行ボタンを押すと QR コードがフォルダに保存されます。

![AppsScript](https://storage.googleapis.com/zenn-user-upload/8867151b66c5-20220712.png)
