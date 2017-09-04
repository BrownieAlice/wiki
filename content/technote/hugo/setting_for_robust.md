---
title: "Robust設定"
date: 2017-09-04T22:36:49+09:00
categories:
 - "Hugo"
weight: 2
---

## Robustについて
[Robust](https://themes.gohugo.io/robust/)はHugoのテーマの1つ. [デモページ](https://themes.gohugo.io/theme/robust/)のようなブログ風のページを作ることができる.  
リポジトリは [https://github.com/dim0627/hugo_theme_robust.git](https://github.com/dim0627/hugo_theme_robust.git) にある.

## 標準設定
HugoやRobust自体の標準で設定する必要のある事.

### タグ/カテゴリ/シリーズの利用
タグ/カテゴリ/シリーズの分類機能を使うには `config.toml` を少し編集してやる必要がある. 以下を追記する.

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
デフォルトだと要約されて表示される記事の文面がかなり長めに表示される. それを日本語用に適切な長さにするためには `config.toml` に以下を追記する.

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

### 画像の挿入
画像はShortcodeを使って挿入できる. 以下を本文中に挿入すれば良い. ただしShortcodeの前後は空行にしておくこと.

```makdown
{{% img src="images/foo/bar.jpg" w="600" h="400" caption="hoge" href="https://example.com" %}}
```

captionとhrefは省略可能.

### disqus導入
[disqus](https://disqus.com/)に登録してshort nameを貰えれば, 後は `config.toml` を以下のように設定するだけ.

```toml
disqusShortname = "hoge"
```

### copyright追加
copyrightを追加する. クリエイティブ・コモンズの画像なり文章なりを無理やり入れることで出来た. `config.toml` を以下のように設定した.

```toml
copyright = "Copyright &copy; 2017-2017, BrownieAlice.<br><a rel='license' href='http://creativecommons.org/licenses/by-sa/4.0/'><img alt='クリエイティブ・コモンズ・ライセンス' style='border-width:0' src='https://i.creativecommons.org/l/by-sa/4.0/80x15.png' /></a><br>このサイトのテキストは原則として <a rel='license' href='http://creativecommons.org/licenses/by-sa/4.0/'>クリエイティブ・コモンズ 表示 - 継承 4.0 国際 ライセンス</a> の下に提供されています."
```

すごい無理やり….

## カスタマイズ
一部はRobustのthemeを直接いじらないと設定できない.

### About追加
右のサイドバーにAboutを追加したい. これをするには `layouts/partials` フォルダを作りそこに `sidebar.html` を追加し中身を以下のようにする.

```html
<aside class="l-sidebar">

  <div class="sections sidebar">
    <section class="sidebar">
      <header>ABOUT</header>
      <div>
	hogehogehoge. <br>
	<div align = "center">
	  <a href = "https://example.com/">foo</a> / 
	  <a href = "https://example.com/">bar</a> / 
	  <a href = "https://example.com/">poyo</a> / 
	  <a href = "https://example.com/">piyo</a>
	 </div>
      </div>
    </section>
    <section class="sidebar">
      <header>LATESTS</header>
      <div>
        <div class="articles sm">
          {{ range $i, $p := (first 10 .Site.Pages) }}
          {{ .Render "li_sm" }}
          {{ end }}
        </div>
      </div>
    </section>

    {{ range $key, $value := .Site.Taxonomies }}
    <section class="sidebar">
      <header>{{ $key | upper }}</header>
      <div>
        <ul class="terms">
          {{ range first 10 $value.ByCount }}<li><a href="{{ $.Site.BaseURL}}{{ $key }}/{{ .Name | urlize }}">{{ .Name }}</a></li>{{ end }}
        </ul>
      </div>
    </section>
    {{ end }}
  </div>

</aside>
```

実はほとんどrobustの `layouts/parials/sidebar.html` のコピー. それを少し改変しただけ. `themes/layouts` より `layouts` のほうが優先されるので`themes` フォルダを直接いじることなくカスタマイズすることができる.

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
