---
title: "Robust設定"
date: 2017-09-04T22:36:49+09:00
tags:
 - "Hugo"
weight: 2
---

## Robustについて
[Robust](https://themes.gohugo.io/robust/)はHugoのテーマの1つ.
リポジトリは [https://github.com/dim0627/hugo_theme_robust.git](https://github.com/dim0627/hugo_theme_robust.git)にある.

## 設定項目
Robustの機能として設定できる項目.

### タグ/カテゴリ/シリーズの利用
タグ/カテゴリ/シリーズの分類機能を使うには `config.toml` を少し編集する必要がある. 以下を追記する.

```toml
[taxonomies]
tag = "tags"
category = "categories"
series = "series"
```

また, 記事にカテゴリを割り当てる際はフロントマターを以下のように設定する.

```yaml
---
title: "hoge"
date: piyo
categories :
 - "po"
 - "popo"
---
```

### 記事要約部分の日本語対応
デフォルトだと記事の要約として表示される分量が異常に多い. 日本語として適切な長さにするためには `config.toml` に以下を追記する.

```toml
hasCJKLanguage = true
```

### サムネイル設定
記事のサムネイルも設定できる.

#### デフォルトサムネイル
お好みの画像をjpgファイルにして `static/images/default.jpg` におけば良い.

#### カスタムサムネイル
各記事に個別にサムネイルを設定することもできる. 記事のフロントマターに以下を追記すれば良い.

```yaml
thumbnail: "images/hoge/thumbnail.jpg"
```

すると `static/images/hoge/thumnail.jpg` がサムネイル画像になる. サムネイル画像自体はpngでもsvgでも入れてくれる. 要はブラウザ側がそのファイル形式に対応できているかによる.

### 目次の割当
記事の最初に目次をつけることもできる. デフォルトでは挿入されないが, フロントマターに以下を追記すると目次が表示されるようになる.

```yaml
toc: true
```

### Author設定
右のサイドバーの著者情報は `config.toml` を編集することで設定できる.
例えば以下のように設定する.

```toml
[params.author]
thumbnail = "images/author.png"
name = "BrownieAlice"
description = """
<div align="center">
工業大学に通う大学院生. <br>
ロボコンに参加したことがあったり? <br>
<a href="https://blog.browniealice.net">Blog</a> /
<a href="https://wiki.browniealice.net">wiki</a> /
<a href="https://github.com/BrownieAlice/blog.git">Repository</a>
</div>"""
github = "https://github.com/BrownieAlice"
```

### 画像の挿入
画像はShortcodeを使って挿入できる. 以下を本文中に挿入すれば良い. ただしShortcodeの前後は空行にしておくこと.

```markdown
{{%/* img src="images/foo/bar.jpg" w="600" h="400" caption="hoge" href="https://example.com" */%}}
```

captionとhrefは省略可能.

### disqus導入
[disqus](https://disqus.com/)に登録してshort nameを貰えれば, 後は `config.toml` を以下のように設定するだけで利用できる.

```toml
disqusShortname = "hoge"
```

### copyright追加
copyrightを追加する. `config.toml` を以下のように設定した.

```toml
copyright = "Copyright &copy; 2017-2017, BrownieAlice.<br><a rel='license' href='http://creativecommons.org/licenses/by-sa/4.0/'><img alt='クリエイティブ・コモンズ・ライセンス' style='border-width:0' src='https://i.creativecommons.org/l/by-sa/4.0/80x15.png' /></a><br>このサイトのテキストは原則として <a rel='license' href='http://creativecommons.org/licenses/by-sa/4.0/'>クリエイティブ・コモンズ 表示 - 継承 4.0 国際 ライセンス</a> の下に提供されています."
```

## カスタマイズ
一部はRobustのthemeを直接いじらないと設定できない.

### favicon追加
iPhone向けのlogoは `static/images/logo.png` を配置すればすむ. ただ普通は林檎みたいな産業廃棄物は使わないのでちゃんと設定しないといけない.  
`layouts/partials` フォルダに `meta.html` を作り中身を以下のようにする.

```html
<meta charset="utf-8">
<meta name="pinterest" content="nopin">
<meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
<meta name="theme-color" content="#263238">
{{ with .Site.Params.contact }}<meta name="contact" content="{{ . }}">{{ end }}
{{ .Hugo.Generator }}

<link rel="apple-touch-icon" href="{{ .Site.BaseURL }}images/logo.png">
<link rel="shortcut icon" type="image/x-icon" href="{{ .Site.BaseURL }}favicon.ico">
{{ with .RSSLink }}<link rel="alternate" type="application/rss+xml" title="RSS" href="{{ . }}">{{ end }}

<link rel="canonical" href="{{ .Permalink }}">
```

そうしたあと `static` フォルダに `favicon.ico` を配置すればアイコンが表示されるようになる.

## 参考サイト
[dim0627/hugo_theme_robust](https://github.com/dim0627/hugo_theme_robust)  
[HUGO のテーマ Robust のカスタマイズ - zzzmisa&#39;s blog](http://blog.zzzmisa.com/customize_hugo_theme/)  
[Github Pages/Hugoでブログを作成してみた - A1 Blog](https://gyoza.beer/post/2017-05-14-start-blog-with-hugo/)  
