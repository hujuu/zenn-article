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
