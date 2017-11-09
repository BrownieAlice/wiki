---
title: "Hugo使い方"
date: 2017-08-15T22:15:02+09:00
tags:
 - "Hugo"
weight: 1
---

## Hugoについて
静的Webページ変換ツールの1つ. 他にも[Jekyll](https://jekyllrb-ja.github.io/)や[Hexo](https://hexo.io/)などが有名.  
基本的にはMarkdownで記事を書き, Hugoを起動して変換してもらって所定の静的なWebページを作る. スタイルなどを調整してやればBlogみたいなWebページもできる.  

## Blogとwikiについて
ブログは[Hugo](https://gohugo.io/)を用いてGitHub Pages上で公開している. Blogは[Alice in the Machine - Blog](https://browniealice.github.io/blog/)にある.  
wikiも同様にしてGitHub Pages上で公開している. wikiは[Alice in the Machine - wiki](https://browniealice.github.io/wiki/)にある.  
## Hugoインストール
[Install Hugo](https://gohugo.io/getting-started/installing)を参考にすれば良い.
Ubuntuのバイナリインストールでいいなら

```bash
snap install hugo
```

で完了.

### Hugo初期設定
既にレポジトリがある場合やる必要はないが, 新たにWebページを作成するなら必要.

```bash
Hugo new site hoge
```

で `hoge` ディレクトリに新たなHugoのワークスペースができる. あとは `hoge` ディレクトリに移動して以下をやっておく.

```bash
git init .
git remote add origin https://github.com/<user-name>/hoge.git
git add *
git commit -m "Init Hugo."
git push -u origin master
```

こうすれば後は普通に記事制作するだけで済む.

### テーマ
Hugoは自分の思うテーマを選んでやらないと, 思ったようなWebページはできない.
テーマは [Hugo Themes|CompleteList](https://themes.gohugo.io/) に一覧になっている. 導入したいテーマが見つかったら

```bash
git submodule add https://github.com/hoge/poyo.git themes/piyo
```

で導入する. あとはそのテーマに合わせてフロントマターや `config.toml` を編集する.  
`config.toml` でテーマを設定するには

```toml
theme = "piyo"
```

とするだけでよい. 他にもテーマ依存の設定などもしていく.

### config.toml
サイトについての情報などは基本的に `config.toml` で設定する.  
例えば以下のようにする.

```toml
baseURL = "hoge"
languageCode = "ja-JP"
title = "poyo"
theme = "piyo"
publishDir = "docs"
disqusShortname = "foo"
copyright = "Copyleft; 2017-2017, BrownieAlice. All rights reversed."
```

### GitHub Pages
Github Pagesを用いてWebサイトを公開するなら少し設定を変更する必要がある.

#### GiuHub側
リポジトリのページを開いて `Settings -> GutHub Pages -> Source -> master branch /docs folder` と設定して保存する.  
するとmasterブランチの `docs` フォルダ 以下がWebページだとみなしてWebサイトにしてくれる.

#### Hugo側
`config.toml` を少しいじる.

```toml
publishDir = "docs"
```

こうすると `docs` フォルダ以下にHTMLなどが生成されるようになる.

## 環境構築
### Blog
すでに中身はgit上で公開しているので

```bash
cd ~/Document
git clone https://github.com/BrownieAlice/blog.git
cd blog
git submodule update --init
```

でよい.
あとは
`~/Document/blog`
上で作業する.

### wiki
Blogと同様.

```bash
cd ~/Document
git clone https://github.com/BrownieAlice/wiki.git
cd wiki
git submodule update --init
```

で, あとは `~/Document/wiki` 上で作業する.

## 記事の制作
以下が一連の動作.

```bash
hugo new post/hoge.md
hugo server -D -w
hugo undraft content/post/hoge.md
hugo
git add *
git commit -m "new post hoge"
git push origin master
```

`hugo server`
でローカルにサーバーを立ち上げてブラウザ上で確認できるようになる.
Blogなら [http://localhost:1313/blog](http://localhost:1313/blog) がそのリンク先.  
そして下書き記事を正式な記事にしてコンパイルし変更をコミットする.

### カテゴリー
カテゴリとして`po`と`popo`を追加する場合, 記事のフロントマターを以下のように設定する.

```yaml
---
title: "hoge"
date: piyo
categories :
 - "po"
 - "popo"
---
```

## 参考サイト
- [GitHub Pagesでブログ立ち上げ - Hugoを使う](https://www.kaitoy.xyz/2015/08/28/using-hugo/)
- [GitHub Pagesの新機能、ソース設定が地味にいい](https://www.kaitoy.xyz/2016/08/18/simpler-github-pages-publishing/)
- [静的サイトジェネレータHugoを使ったサイト構築（コンテンツ編１）](http://staff.feedtailor.jp/2016/05/18/hugo_06/)
- [HugoとGitHub Pagesで静的サイトを公開する - Qiita](http://qiita.com/satzz/items/e24bd703fc04fb45f7ef)
- [Hugo + Github Pagesでブログを公開してみた - Qiita](http://qiita.com/eichann/items/4fe61b8b9bbafcfbe847)
