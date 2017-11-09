---
title: "DocDock設定"
date: 2017-11-09T10:30:47+09:00
draft: true
tags:
 - "Hugo"
weight: 3
---

## DocDockについて
[DocDock](https://themes.gohugo.io/docdock/)はHugoのテーマの1つ. [デモページ](https://themes.gohugo.io/theme/docdock/)のようなドキュメント用のページを作ることができる.  
リポジトリは[https://github.com/vjeantet/hugo-theme-docdock.git](https://github.com/vjeantet/hugo-theme-docdock.git)にある.

## 標準設定
HugoやDocDock自体の標準で設定する必要のある事.

### タグ/カテゴリ/シリーズの利用
上手く行かないので諦めよう. タグは左上にリンクが作成されるがリンク先の実体が生成されない. カテゴリはリンク先の実体は生成されるが自動でそのページにリンクは貼られない. シリーズは試してないけどwikiにはあわなそう.

### メニューバーの追加
メニュー場にリンクを使いするには例えば以下を `config.toml` に記載する.

```markdown
[[menu.shortcuts]]
  pre = "<h3>Links</h3>"
  identifier = "links"
[[menu.shortcuts]]
  name = "Blog"
  url = "https://blog.browniealice.net"
  weight = 30
  identifier = "blog"
```

## カスタマイズ
一部はDocDockのthemeを直接いじらないと設定できない.
左のバーにコピーライトを追加したい. これをするには `layouts/partials` フォルダを作り `menu-footer.html` を追加し中身を以下のようにする.

```markdown
Copyright &copy; 2017-2017, BrownieAlice.<br>
<a rel='license' href='http://creativecommons.org/licenses/by-sa/4.0/'><img alt='クリエイティブ・コモンズ・ライセンス' style='border-width:0' src='https://i.creativecommons.org/l/by-sa/4.0/80x15.png' /></a><br>
このサイトのテキストは原則として <a rel='license' href='http://creativecommons.org/licenses/by-sa/4.0/'>クリエイティブ・コモンズ 表示 - 継承 4.0 国際 ライセンス</a> の下に提供されています.
```
