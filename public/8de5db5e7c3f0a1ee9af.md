---
title: Google Apps Script で簡単なサイトを作成してみる
tags:
  - GoogleAppsScript
private: false
updated_at: '2018-12-08T07:00:54+09:00'
id: 8de5db5e7c3f0a1ee9af
organization_url_name: supership-inc
slide: false
ignorePublish: false
---
[Supership株式会社 Advent Calendar 2018 - Qiita](https://qiita.com/advent-calendar/2018/supership) の8日目の記事になります。

Supership株式会社 の @tanjo です。[Supership株式会社 Advent Calendar 2016 - Qiita の15日目](https://qiita.com/tanjo/items/3fd248b3302364e13958) 以来の Advent Calendar 投稿となります。

今回は [G Suite Developer Hub](https://script.google.com/home) の提供が始まった記念に [Google Apps Script](https://developers.google.com/apps-script/) で簡単な Web サイトを作成しようと思います。

## G Suite Developer Hub とは

元「Apps Scriptダッシュボード」。

管理が面倒だった Apps Script をまとめて表示したり、エラー率や実行数もチェックができる管理ツールです。

かなりデザインが綺麗になりました。

## Google Apps Script とは

Google Apps Script [^0] は Microsoft Excel の VBA にあたります[^1]。
JavaScript ベースの言語なので JavaScript 経験者であれば比較的簡単に書くことができます。

## プロジェクトを作る

Apps Script はプロジェクト単位で管理します。
スクリプトそのものは「gs ファイル」に書きます。
それ以外に、HTMLファイルを作成することができます。

![image01.png](https://qiita-image-store.s3.amazonaws.com/0/30241/e026d2df-2642-8dfe-c1e5-f6feffee2ffe.png)

## 実際にWebサイトを作る

### 1. まず、[G Suite Developer Hub](https://script.google.com/home) に移動

### 2. 「新規」 - 「新規スクリプト」を押して無題のプロジェクトを作成

![image23.png](https://qiita-image-store.s3.amazonaws.com/0/30241/6b0d89cc-cbd4-a794-2ad6-f4529bf77603.png)

### 3. 無題のプロジェクトは以下のように「コード.gs」だけがある状態を確認

![image12.png](https://qiita-image-store.s3.amazonaws.com/0/30241/24ec2379-e77d-d86e-9922-a18f118ab393.png)

### 4. プロジェクト名を設定

「無題のプロジェクト」だと何をするためのスクリプトかわからないので変更します。
左上の「無題のプロジェクト」をクリックすると変更できます。

![image13.png](https://qiita-image-store.s3.amazonaws.com/0/30241/c470d1a0-ee41-6f58-2f92-9dcca97728ab.png)

Goolge Drive から見ると以下のとおりプロジェクト名が表示されます。

![image15.png](https://qiita-image-store.s3.amazonaws.com/0/30241/cfc7498f-d257-ba52-ca14-04a5bb966b6b.png)

### 5. HTMLファイルを作成

「ファイル」 - 「新規作成」 - 「HTML ファイル」 をクリックします。

![image.png](https://qiita-image-store.s3.amazonaws.com/0/30241/0efb7c6b-7571-0340-f229-8b29b6df3888.png)

するとファイルを作成するポップアップが表示されます。

![image.png](https://qiita-image-store.s3.amazonaws.com/0/30241/5a13be79-1dc7-86f7-b9f6-a4c4029d5728.png)

今回はファイル名を「index.html」にします。

```html:index.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title><?= title ?></title>
  </head>
  <body>
    <div class="content">
      <span class="welcome">
        Welcome to ようこそ昔風ホームページへ！
      </span>
    </div>
  </body>
</html>
```

### 6. main.gs を編集

Google Apps Script で Webサイトを表示するためには、
リクエストに反応してHTMLファイルを読み込む必要があります。

難しく感じるかもしれませんが、たった3行で書くことができます。

```js:main.gs
function doGet(e) {
  const indexHtml = HtmlService.createTemplateFromFile('index.html');
  indexHtml.title = 'ようこそ！';
  return indexHtml.evaluate();
}
```

doGet は Java Servlet を彷彿とさせます[^3]。
GET リクエストを受け取るには `doGet` 関数の中に処理を書く必要があります。
HTML ファイルを読み込むには `HtmlService` を利用します。
2行目の `indexHtml.title ...` は HTML ファイル内の `<?= title ?>` を置換するためのものです。
今回の場合はタイトルが変更されています。

### 7. Webサイトを公開



Webサイトを公開するにはバージョンを管理して、
作成したバージョンをウェブアプリケーションとして導入する必要があります。

#### ⅰ. 版を管理

バージョンは「ファイル」-「版を管理」から作成することができます[^2]。

![image18.png](https://qiita-image-store.s3.amazonaws.com/0/30241/9b701866-3565-6979-feb7-50128319a0b1.png)

説明を記載して、「新しいバージョンを保存」を押すとバージョン管理をすることができます。

![image19.png](https://qiita-image-store.s3.amazonaws.com/0/30241/337fe345-8e69-5019-8ca6-c22377540f72.png)

#### ⅱ. ウェブアプリケーションとして導入

次に公開したいバージョンをウェブアプリケーションとして導入します。

「公開」-「ウェブアプリケーションとして導入...」を押す。

![image16.png](https://qiita-image-store.s3.amazonaws.com/0/30241/9984a693-fdb4-834a-07be-c9d56f8ab1c0.png)

例では初回なので「新規作成」になっていますが、この画面から説明を入力するだけで新たなバージョンを作成することができます。
また、「版を管理」で作成したバージョンは「プロジェクト バージョン」で選択すると適用されます。

![image17.png](https://qiita-image-store.s3.amazonaws.com/0/30241/a8ae4f61-de0d-5e1f-881e-1cea40e268dd.png)

「導入」ボタンを押せば完了。

「次のユーザーとしてアプリケーションを実行」は誰のアカウントでWebサイトの表示処理を行うか指定できます。
「アプリケーションにアクセスできるユーザー」は公開範囲になります。 G Suite を利用していると社内だけしか公開できない可能性もあるので注意してください。

テストで作成する場合はデフォルト設定で問題ありません。

### 8. アクセスする

「導入」ボタンを押した後に URL が表示されます。

![image21.png](https://qiita-image-store.s3.amazonaws.com/0/30241/5fd4f599-788c-7802-0218-f98742f3feea.png)

この URL にアクセスすると作成した Web ページが表示されます。

![image22.png](https://qiita-image-store.s3.amazonaws.com/0/30241/919a67d1-f895-02f2-dd56-7b1960455641.png)

## 上級編

さて、HTMLファイルを表示するところまで終わったのであとは **ぼくのかんがえたさいきょうのうぇぶさいと** を作るだけです。

Google Apps Script ではスプレッドシートが簡単に操作できることので、疑似DBとして扱うことができます。これを利用することで動的なウェブサイトの表示を行うことができます。

### アクセスカウンターを設置

一番楽そうなアクセスカウンターを設置してみます。

必要なものはスプレッドシート と Apps Script だけ。

前提条件として、利用するスプレッドシートのIDを`hogeFugaPiyo`、シート名を`DATA`とします。

アクセス数をスプレッドシートで管理し、動的に変更することができます。

```js:main.gs
function doGet(e) {
  addVisitor();
  const indexHtml = HtmlService.createTemplateFromFile('index.html');
  indexHtml.title = 'ようこそ！';
  indexHtml.visitors = getVisitors();
  return indexHtml.evaluate();
}
```

```js:sheet.gs
var spreadsheet = SpreadsheetApp.openById('hogeFugaPiyo');
var sheet = spreadsheet.getSheetByName('DATA');

// セルの範囲
function getVisitorsRange() {
  return sheet.getRange(1, 2);
}

// 訪問者数を取得する
function getVisitors() {
  return getVisitorsRange().getValue();
}

// 訪問者数を増やす
function addVisitor() {
   getVisitorsRange().setValue(getVisitors() + 1);
}
```

```html:partial.html
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title><?= title ?></title>
    <style> /* CSS省略 */ </style>
  </head>
  <body>
    <div class="content">
      <span class="welcome">
        Welcome to ようこそ昔風ホームページへ！
      </span>
      <div class="visitors">
        あなたは <?
          var splitedVisitors = ('' + visitors).split('');
          splitedVisitors.forEach(function(num) {
            output.append('<span class="frame">' + num + '</span>');
          });
        ?> 人目の訪問者です
      </div>
      <marquee>
        画像は <a href="https://www.irasutoya.com/">いらすとや</a> さんのものを利用しています。
      </marquee>
      <div class="image">
        <img src="https://4.bp.blogspot.com/-qjYA1Wk5BPE/UYurYUqeFNI/AAAAAAAARzQ/sUkiZZkQSIM/s400/kouyou_cat.png">
      </div>
    </div>
  </body>
</html>
```

これでアクセスカウンターの完成です。
リロードするとカウントが増えていくのがわかります。

![image03.gif](https://qiita-image-store.s3.amazonaws.com/0/30241/3d12d490-81cc-fc1c-b965-26c5cb78c8a7.gif)

## 完成！

いろいろ改造していけば、とても素敵なWebサイトを Google Apps Script で公開することができます。

![image02.png](https://qiita-image-store.s3.amazonaws.com/0/30241/d0619c9c-9b3f-2df1-c371-cd5a9981e8a3.png)

このWebサイトを表示するコードを以下に置いておきます。

- [tanjo/gas\-website: Google Apps Script でウェブサイト制作](https://github.com/tanjo/gas-website)

今回は基本しか紹介しませんでしたが、 Goolge の API を利用すれば Google カレンダーを TODO リストにしてしまうことも簡単にできます。

- [tanjo/google\-calendar\-todo: Google カレンダーの colorId を利用した ToDo リストを表示する GAS](https://github.com/tanjo/google-calendar-todo)

やってみると結構楽しいので皆さんもいろいろ試してみてはどうでしょうか？

[^0]: 以降、 Apps Script として表記する。
[^1]: Excel の VBA と違い、他にも Apps Script でカレンダーや Gmail を 操作することができます。
[^2]: 「版を管理」と書かれていますが、「バージョン管理」と読み替えるとわかりやすいです。
[^3]: POSTメソッドが使いたい場合は doPost もあります。
