#  ヘルパーライブラリを用いた外部への音声通話発信

この手順ではTwilio Twilio RESTを利用し外部への音声通話発信を行います。

## 2-1: ヘルパーライブラリを初期化

`OutboundCall.js`を開き、ヘルパーライブラリを初期化します。この際、先ほど環境変数に設定した`Account Sid`および`Auth Token`を使用します。

```js
// 2-1: Twilioヘルパーライブラリを初期化
const twilio = require('twilio');
const client = new twilio(TWILIO_ACCOUNT_SID, TWILIO_AUTH_TOKEN);
```

## 2-2: 静的なTwiMLを用いて外部に発信 

続けてこの`client`を用いてTwilio番号から認証された番号に発信します。この際に自動でメッセージを流すように`twiml`を設定します。

```js
// 2-2: 静的なTwiMLを用いて外部に発信
client.calls.create({
    from: TWILIO_NUMBER,
    to: MY_PHONE_NUMBER,
    twiml: '<Response><Say language="ja-JP">登録された番号に発信しています。</Say></Response>'
}).then(call => console.log(call.sid))
  .catch(err => console.log(err));
```

次のコマンドを実行し、ご自身の電話で設定されたメッセージが流れることを確認してください。

```zsh
node OutboundCall.js
```

ここで `to`を登録されていない番号に設定した場合は、エラーが返されます。


## 2-3: 動的にTwiMLを構築し外部に発信 

動的にTwiMLを作成し外部に発信してみましょう。直前のステップで実装した箇所をコメントアウトし、次のコードに差し替えてください。

```js
// 2-3: 動的にTwiMLを構築し外部に発信
// VoiceResponseを使用
const VoiceResponse = twilio.twiml.VoiceResponse;
const response = new VoiceResponse();
response.say('動的に生成された内容で発信しています。', {language: 'ja-JP'})

//生成したTwiMLを使用し発信
client.calls.create({
    from: TWILIO_NUMBER,
    to: MY_PHONE_NUMBER,
    twiml: response.toString()
}).then(call => console.log(call.sid))
  .catch(err => console.log(err));
```

再度`OutboundCall.js`を実行し結果を確認してください。

## 次のハンズオン

[ハンズオン: ブラウザーフォンを体験](../04-Experience-Browser-Phone/00-Overview.md)