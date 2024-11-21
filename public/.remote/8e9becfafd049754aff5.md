---
title: Github の Private リポジトリ を Maven リポジトリ として使う
tags:
  - Android
  - Maven
  - GitHub
  - gradle
private: false
updated_at: '2018-04-06T18:28:48+09:00'
id: 8e9becfafd049754aff5
organization_url_name: supership-inc
slide: false
ignorePublish: false
---
3年前くらいに [Github Pages を利用して Maven リポジトリを構築する方法](https://qiita.com/tanjo/items/844d0444d7ffa541120e) を Qiita に書いた.

今回は Private リポジトリでやってみる.
利用者にプラグインの強要をしないようにしたいので 「gradle-git-repo-plugin」 等をプラグインを使わずに Github の機能だけでダウンロードできるようにする.

## はじめに

私は Android エンジニアなので、 Android Studio の build.gradle をターゲットとして書く.

基本はパブリックに置くのと同じなので AAR ファイルの生成とかは省略する.

## ライブラリ側

- プライベートリポジトリ
- ライブラリのユーザーには Read 権限を付与
- リポジトリは **hoge/android-sdk** に配置
- groupId は **jp.hoge.library**
- artifactId は **android-private-sdk**
- version は **1.0.0**

## アプリ側

### 1. Github の [Personal Access Token](https://github.com/settings/tokens) を生成

権限は `repo` を設定すればよい. 

### 2. プロパティファイルの作成

取得した Personal Access Token と Github のユーザー名を Gradle で使えるようにする.

```:github.properties
username=USERNAME
token=GITHUB_PERSONAL_ACCESS_TOKEN
```

セキュリティを考慮して Personal Access Token はユーザーごとに用意しよう.

`.gitignore` に以下を追加することをオススメする.

```:.gitignore
github.properties
```

### 3. Maven リポジトリを追加する

- URL は `https://raw.githubusercontent.com` を利用
- `credentials` に先程のプロパティファイルの情報を設定
- `authentication` を設定 （ベーシック認証）

```gradle:build.gradle
allprojects {
    repositories {
        google()
        jcenter()
        maven {
            def propsFile = rootProject.file('github.properties')
            def props = new Properties()
            props.load(new FileInputStream(propsFile))
            url "https://raw.githubusercontent.com/hoge/android-sdk/master"
            credentials {
                username props['username']
                password props['token']
            }
            authentication {
                basic(BasicAuthentication)
            }
        }
    }
}
```

### 4. 依存関係を記述する

```gradle:build.gradle
dependencies {
  implementation "jp.hoge.library:android-private-sdk:1.0.0"
}
```

## おわりに

2段階認証をしている方でも利用できます.
トークンは利用しなくなったら必ず **Revoke** しよう.
