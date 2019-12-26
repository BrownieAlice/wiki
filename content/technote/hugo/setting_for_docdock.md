---
title: "DocDock設定"
date: 2017-11-09T10:30:47+09:00
tags:
 - "Hugo"
weight: 3
---

## DocDockについて
[DocDock](https://themes.gohugo.io/docdock/)はHugoのテーマの1つ. [デモページ](https://themes.gohugo.io/theme/docdock/)のようなドキュメント用のページを作ることができる.  
リポジトリは[https://github.com/vjeantet/hugo-theme-docdock.git](https://github.com/vjeantet/hugo-theme-docdock.git)にある.

## 設定項目
DocDockの機能として設定できる項目.

### タグ/カテゴリ/シリーズの利用
タグを利用するなら `config.toml` に
```toml
[taxonomies]
  tag = "tags"
```
と設定した上で, 各ページのフロントマター部に
```yaml
tags:
 - "Hugo"
```
と記載すれば良い.  
日本語記事が主な場合, タグの一覧ページの概要を見やすくするために `config.toml` に
```toml
hasCJKLanguage = true
```
と設定したほうが良い.  

### メニューバーの追加
最上部メニューバーにリンクを追加するには, `config.toml` に以下のように付け加えればよい.

```toml
[[menu.shortcuts]]
  name = "Blog"
  url = "https://blog.browniealice.net"
  weight = 30
  identifier = "blog"
[[menu.shortcuts]]
  name = "wiki"
  url = "https://wiki.browniealice.net"
  weight = 31
  identifier = "wiki"
```

## カスタマイズ
一部はDocDockのthemeを直接いじらないと設定できない.  
左のバーにコピーライトを追加した場合, `layouts/partials` フォルダを作り `menu-footer.html` を追加し中身を以下のようにする.

```html
<p style="text-align:center;">
  Copyright &copy; 2017-2019, BrownieAlice.<br>
  <a rel='license' href='http://creativecommons.org/licenses/by-sa/4.0/'>
    <img alt='クリエイティブ・コモンズ・ライセンス' style='border-width:0'
      src='https://i.creativecommons.org/l/by-sa/4.0/80x15.png' />
  </a>
</p>
```
