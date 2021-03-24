---
title: "WSL2のパス周りを見直した話"
emoji: "📝"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","wsl"]
published: false
---

# WSLが最近やけに重い

私は、Windows で開発するときに WSL2(Windows Subsystem for Linux 2)の中で作業することが多いです。Windows の中で簡単に Linux の環境を構築できるので気に言って使っているのですが、最近 WSL2 が思いと感じることがあります。前にシェルのカスタマイズをしている際に Macbook と並行して作業していたのですが、Macbook と比べてシェルのログイン時の挙動、コマンドの結果が返ってくるまでにかかる時間など目に見えて遅く感じることが多くありました。OS が違う時点で比較するのはナンセンスだと感じますが、WSL2 を使うときだけやけに重くなるのが気になったため、パスを見直したところ解決策が見つかったのでまとめます。

# WSLの有名なパスの話

Windows 上で動作する WSL2 ですが、WSL2 から Windows 側のファイルにアクセス、また逆も簡単にできるのは有名です。
例えば、WSL2 のターミナル上にて以下のコマンドを実行すると Windows のエクスプローラーが立ち上がります。

```
$ explore.exe
```

上のコマンドにディレクトリのパスを指定すると指定したディレクトリをエクスプローラーで開いてくれます。

また VScode が Windows にインストールされている場合、code コマンドを使うことで VScode でファイルを開けます。

```
$ code .
```

で WSL2 のカレントディレクトリが VSCode 上で開かれます。

この部分については以下のスクラップにあわせてまとめました。

https://zenn.dev/ryuu/scraps/74c6db6e9347e4

しかし、なぜこんなに簡単にファイル連携ができるのでしょうか。答えは簡単でパスが関係しています。