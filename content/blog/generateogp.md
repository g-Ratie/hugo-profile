---
title: tcardgenとGithub ActionsでOGPを自動生成する
date: 2024-03-20T04:36:51+09:00
author: ["ラティ"]
categories: ["雑記"]
tags: ["blog"]
cover:
  image: "images/ogp/generateogp.png"
  alt: "tcardgenとGithub ActionsでOGPを自動生成する"

ShowPostNavLinks: true

---
## 事前準備

まず、tcardgenをインストールします。

```bash
$ go get github.com/Ladicle/tcardgen
```

また、フォントファイルをstatic/font以下に配置する必要があります。
今回はReadmeで使用されているKintoフォントをインストールしています。




## 画像生成
tcardgenはHugoのFront Matterを参考にしているため、Markdownファイルにメタデータを記述する必要があります。
自分はHugoのテーマにPaperModを使用しているため、Front Matterの記述は以下のようになっています。
```markdown
title: tcardgenとGithub ActionsでOGPを自動生成する
date: 2024-03-20T04:36:51+09:00
author: ["ラティ"]
categories: ["雑記"]
tags: ["blog"]
cover:
  image: "images/ogp/generateogp.png"
  alt: "tcardgenとGithub ActionsでOGPを自動生成する"
```

メタデータを記述したら、tcardgenコマンドを実行してOGP画像を生成します。

```bash
$ tcardgen -f static/font/kinto -o static/images/ogp -t static/ogp/template/OGP.png content/**/*.md
```

実行時にはオプションとして「-f（フォントディレクトリ）」・「-o（出力先ディレクトリ）」・「-t（テンプレート画像）」を指定しています。


## 画像のカスタマイズ

画像のスタイルは、tcardge.yamlで設定できます。
これはtcardgenにサンプルがあるので、それを参考にしました。

https://github.com/Ladicle/tcardgen/blob/master/example/template3.config.yaml

```
title:
  start:
    px: 130
    py: 200
  fontSize: 68
  fontStyle: Bold
  maxWidth: 946
  lineSpacing: 10

info:
  start:
    px: 240
    py: 470
  fontSize: 35
  fontStyle: Regular
  separator: " - "

tags:
  start:
    px: 950
    py: 470
  fgHexColor: "#FFFFFF"
  bgHexColor: "#7F7776"
  fontSize: 35
  fontStyle: Medium
  boxAlign: Left
  boxSpacing: 6
  boxPadding:
    top: 6
    right: 10
    bottom: 6
    left: 8
```


自分のケースでは、タイトル・アカウント名・タグのデザインを少しだけカスタマイズしています。

## Github Actionsでの自動生成

tcardgenコマンドをGitHub Actionsで実行することで、コミットの度にOGP画像を自動生成できます。


以下が実際のコード
```
- name: Generate OGP images
run: tcardgen -f static/font/kinto -o static/images/ogp -t static/ogp/template/OGP.png -c static/ogp/template/OGP.config.yaml content/**/*.md
```
## 終わりに
Github Actions側で自動生成してもらうことで、OGP画像の生成を自動化できました。
今回はほぼテンプレートそのままですが、テンプレートをもっといい感じに自己流にカスタマイズしていきたいです。
