---
title: "【WSL2】Ubuntu 20.04.4 LTSを22.04 LTSへアップグレードした"
emoji: "💬"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["wsl2", "ubuntu", "ubuntu2204"]
published: false
---

# はじめに

先日、2022年4月21日にUbuntu 22.04 LTS Jammy Jellyfishがリリースされました。これは2020年4月23日にリリースされたUbuntu 20.04 LTSに続くLTSリリースとなります。
現在WSL2（Windows Subsystem for Linux）では[18.04 LTS](https://www.microsoft.com/ja-jp/p/ubuntu-1804-lts/9n9tngvndl3q?activetab=pivot:overviewtab)と[20.04.4 LTS](https://www.microsoft.com/ja-jp/p/ubuntu-20044-lts/9mttcl66cpxj?activetab=pivot:overviewtab)のLTSリリースがMicrosoft Storeにて提供されています。

記事執筆現在`wsl --list --online`で確認できるディストリビューションリストには表示されていませんが、Microsoft Storeには掲載されております。
以下のページからUbuntu 22.04 LTSを新規にダウンロードしWSL2上で実行できます。

https://www.microsoft.com/ja-jp/p/ubuntu-2204-lts/9pn20msr04dw?activetab=pivot:overviewtab

新規にインストールすることはもちろん可能なのですが、現在使用しているLTS環境を引き続ぐ形でバージョンをアップグレードすることも可能となります。
今回はWindowsで開発する際にメイン利用していたUbuntu 20.04.4 LTSを早速22.04 LTSへアップグレードしてみたので備忘録としてまとめていきます。

:::message
アップグレードに失敗する可能性を想定して事前に重要なファイルをバックアップすることをおすすめします。
基本的にアップグレードは自己責任となります。新規に22.04LTSをインストールしたほうが実現性が高いため新しい環境で始める際はアップグレードではなく、22.04 LTSの新規インストールをおすすめします。
:::
# アップグレードのインストール準備

アップグレードを行う前に、現在実行しているバージョンの把握とパッケージの依存関係を最新にし解決しておきます。

## 現行バージョンを確認

まず、現在実行しているUbuntuのバージョンを確認しておきます。

WSL2にてUbuntuを起動し、バージョン情報が記述されたファイルを'cat'で確認します。Ubuntuは/etc/os-releaseの情報を参照することでバージョンを把握できます。

```shell
$ cat /etc/os-release

NAME="Ubuntu"
VERSION="20.04.4 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.4 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```

バージョン情報が確認できました。上の例だと20.04.4 LTS （Focal Fossa）が実行されていることが分かります。

## パッケージの依存解決

Ubuntuにインストールしているパッケージのアップデートと依存解決を行います。

```shell
$ sudo apt update & sudo apt upgrade
```

上記のコマンドでパッケージのアップデートとアップデートを同時に行い、パッケージを最新の状態にしておきます。

## ディストリビューションアップデートの確認

ディストリビューションのアップグレードを確認するために必要なパッケージをインストールします。

```shell
$ sudo apt dist-upgrade & sudo apt install update-manager-core
```

ディストリビューションの依存関係の解決が行われます。`apt install update-manager-core`では、update-manager-coreが存在しなかった場合のみインストールが行われます。

## relese-upgradeの設定

relese-upgradeの設定ファイルを変更します。以下のファイルをエディタで開きます。使用するエディタはVimやnanoなど普段使用しているもので問題ありません。

```shell
# sudoでファイルを開く
$ sudo vim /etc/update-manager/release-upgrades
```

ファイルの一番下に`Prompt`という項目があります。ここの値を`lts`に変更します。18.04 LTSや20.04 LTSなどのLTSリリースを使用している場合はデフォルトで`Prompt=lts`となっているため変更する必要はありません。

設定を確認しファイルを閉じます。値を変更した場合はファイルを保存して閉じるようにします。
これで最新のLTSへアップグレードする準備が完了しました。

# アップグレードの実行

ここまでで最新のLTSへのアップグレード準備が完了しているため、アップグレードの実行に着手していきます。アップグレードの前に重要なファイルや個人設定をバックアップしておくのがおすすめです。

上の設定を行ったあとに以下のコマンドを実行するだけでアップグレード処理が始まります。

```shell
sudo do-release-upgrade -d
```

アップグレードに伴うパッケージの依存関係の解決が行われていくので完了するまで待ちます。処理が一旦完了した際に以下のような同意を求められます。

```shell
アップグレードをインストールするのに数時間かかることがあります。
ダウンロードが完了してしまうと、処理はキャンセルできません。

 続行する[yN]  詳細 [d]
 ```

 `d`で変更が行われる箇所の詳細情報を確認できます。`y`でダウンロードとアップグレードに同意します。警告がでているように、アップグレード開始後のキャンセルは非推奨となるため、ここでアップグレードにて削除される項目、追加される項目について目を通しておくことをおすすめします。

 アップグレードのインストールにはかなり時間がかかる場合はあるので気長に待ちます。途中でパッケージの削除について以下のような確認を求められた場合は`y`で確認して続行します。

 ```shell
 サポートが中止された（あるいはリポジトリに存在しない）パッケージを削除しますか？


71 個のパッケージが削除されます。
```

サポートされなくなったパッケージが削除されたのちにアップグレードは完了します。

```shell
システムのアップグレードが完了しました。

再起動が必要です

アップグレードを完了するには再起動が必要です。
'Y' を選択すると再起動します。
```

上の表示がでればアップグレードは完了しています。

画面の指示に従い`y`と入力してシェルの再起動を行います。シェルの再起動に失敗する場合は、Windows Terminalなどで再度開きなおすことで解決します。

# アップグレード後のバージョンを確認

正常にアップグレードできているかバージョンを確認します。アップグレード前と同じようにUbuntuの場合は`cat /etc/os-release`で確認できます。

```shell
PRETTY_NAME="Ubuntu 22.04 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04 (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

`VERSION="22.04 (Jammy Jellyfish)"`となっていることが確認できれば22.04 LTSへのアップグレードは問題なく完了しております。

# おわりに