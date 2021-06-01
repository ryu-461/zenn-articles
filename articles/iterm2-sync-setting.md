---
title: "iTerm2の設定をDropboxで共有して同期する"
emoji: "📁"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["iterm2","dropbox"]
published: false
---

# はじめに

[iTerm2](https://iterm2.com/) 使ってますか？

iTerm2 とは Mac で使用できるターミナルアプリです。エンジニア界隈では、かなり有名なので現在使用している人でも多いのではないしょうか。

iTerm2 はカスタマイズ機能にも優れており、自分の使いやすいターミナル環境を作ることができます。また、いつも使うツールになるので設定を常にバックアップしてデータを管理し、いつ環境が変わっても同じ設定を使いことが往々にしてあります。

今回、iTerm2 とクラウドストレージサービスの [Dropbox](https://www.dropbox.com/ja/) を使用し、設定を管理、同期する方法について紹介します。

# 設定ファイルの保存先をDropboxにする

iTerm2 を起動して、`設定(Preferences)` を開きます。メニューから `iTerm2 → Preferences` またはショートカットキーで `⌘ + ,` とすることで開くことができます。

設定ウインドウが開いたら `Generalタブ` から `Preferences` を選択します。

![設定ファイル保存場所設定の画像](https://storage.googleapis.com/zenn-user-upload/c18c98bb50725c47100a6202.png)
*設定ファイルの保存先選択*

iTerm2 は設定ファイルの保存先をカスタムでき、自由に保存先を指定できます。

Dropbox のアプリを Mac にインストールして使っている場合、Finder から Dropbox へアクセス可能となり、Dropbox 上の場所も指定できるようになります。

https://help.dropbox.com/ja-jp/installs-integrations/desktop/locate-dropbox-folder

[Dropbox を公式サイトからインストール](https://www.dropbox.com/install)し、セットアップを進めることで Dropbox ディレクトリにアクセス可能となります。

```shell
# ユーザーディレクトリからDropboxへ移動
$ cd ~/dropbox
```

上のようにすることで Dropbox へアクセスできます。Dropbox を開けることを確認したら iTerm2 の設定ファイルの保存場所を作成します。

```shell
# Mac/Settingsディレクトリを作成(任意)
$ mkdir Mac/Settings
```

場所はどこでも大丈夫なのですが、私は他のアプリの設定も合わせて保存する場所として上のようなディレクトリ構成にしています。

ディレクトリを作成したら iTerm2 の設定ファイルの保存先を指定して保存します。

iTerm2 の Preference から `Load preferences from a custom folder or URL` にチェックを入れると Finder が開くので、先程 Dropbbox に作成したファイルの保存場所を選択しセットします。

ディレクトリのセットが完了し ✅ の表示がでれば設定完了です。

# おわりに