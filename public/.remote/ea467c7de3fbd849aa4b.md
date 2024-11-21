---
title: "Slack の \U0001F363 にリンクを貼る"
tags:
  - GoogleAppsScript
  - sushi
  - Slack
private: false
updated_at: '2020-04-09T18:23:58+09:00'
id: ea467c7de3fbd849aa4b
organization_url_name: supership-inc
slide: false
ignorePublish: false
---
# テキストフォーマット

```
<https://supership.jp/|🍣>
```

`< | >` の中の
左に遷移させたい URL を
右に表示させたい文字列を
入れるだけ

# パラメータ 

`parse` を `none` 
`as_user` を `true` 

にするだけ

# 注意点

API 経由以外での投稿方法は知りません

# Tester

ここから簡単に投稿できます

https://api.slack.com/methods/chat.postMessage/test

# 

# Google Apps Script でやる場合

```js
var SLACK_URL_ROOMS_MESSAGE = "https://slack.com/api/chat.postMessage";
var AUTH_TOKEN = "<Slack の API トークン>";

function chatPostMessage() {
  var payload = {
        'token' : AUTH_TOKEN,
        'channel' : '#general',
        'text' : '<https://supership.jp/|🍣>',
        'parse' : 'none',
        'as_user' : true
  };
  var params = {
        'method' : 'post',
        'payload' : payload
  };
  var response = UrlFetchApp.fetch(SLACK_URL_ROOMS_MESSAGE, params);
}
```

# 自動書式設定

自動書式設定を利用すると新たにハイパーリンクを作成できます

<a href="https://slack.com/intl/ja-jp/help/articles/202288908-%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8%E3%81%AE%E6%9B%B8%E5%BC%8F%E8%A8%AD%E5%AE%9A">メッセージの書式設定 | Slack</a>

- テキストを選択し、書式設定ツールバーのリンクアイコンをクリックする
- テキストを選択し、`⌘ + Shift + U` (Mac) か `Ctrl + Shift + U` (Windows/Linux) を押す 

すると以下のようなダイアログが表示されます

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30241/c2fec428-0506-28b6-9f62-e020f7701e3b.png)

テキストとリンクを記載して*Save*すればハイパーリンクの完成

試しに `:sushi:` にこの記事のURLを設定しました

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30241/9c53c870-2aec-9396-700a-a628631bfa9f.png)

ただの文字列になりました

この方法では絵文字にリンクを付与することはできないようです

