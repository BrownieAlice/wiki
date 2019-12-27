---
title: "Yay"
date: 2019-12-27T01:52:11+09:00
tags:
  - "Arch Linux"
---
[Yay](https://github.com/Jguer/yay)はGo言語で書かれたAURヘルパ.  
[yaourt](https://archlinux.fr/yaourt-en)は現在メンテナンスされていないため, 代わりのAURヘルパとしてYayがよく用いられているように思う.

## インストール
GitHubのリポジトリに記載されている `README.md` の通り以下を実行すれば良い.

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

## 高速化
Yayを使っているときのボトルネックは, 公式リポジトリのソフトウェアをダウンロードする際に1つずつ順番に行っていくところだろう.  
Yay自体はそこに関する並列化は行わないため, pacmanの代わりに[powerpill](https://xyne.archlinux.ca/projects/powerpill/)を使うようにする必要がある.[^parallel]

### インストール
powerpillや他に必要な[reflector](https://xyne.archlinux.ca/projects/reflector/)とrsyncをインストールする.
powerpillのみAURパッケージ.

```bash
yay -S reflector rsync powerpill
```

### reflector設定
まずreflectorを用いて高速なサーバをいくつか取得する.
日本, 韓国, 香港, 台湾サーバから最新の20個のリストを取得して早い順に6個を表示させている.

```bash
sudo reflector -p rsync -f 6 -l 20 -c JP -c KR -c HK -c TW
```

私の環境では

```text
Server = rsync://ftp.jaist.ac.jp/pub/Linux/ArchLinux/$repo/os/$arch
Server = rsync://ftp.tsukuba.wide.ad.jp/archlinux/$repo/os/$arch
Server = rsync://jpn.mirror.pkgbuild.com/packages/$repo/os/$arch
Server = rsync://archlinux.cs.nctu.edu.tw/archlinux/$repo/os/$arch
Server = rsync://ftp.tku.edu.tw/archlinux/$repo/os/$arch
Server = rsync://hkg.mirror.rackspace.com/archlinux/$repo/os/$arch
```

以上のような結果が得られた.
これらのサーバをパッケージの更新の際に利用するために, `/etc/powerpill/powerpill.json` 内の `rsync` の `servers` の項目を

```text
"servers": ["rsync://ftp.jaist.ac.jp/pub/Linux/ArchLinux/$repo/os/$arch",
    "rsync://ftp.tsukuba.wide.ad.jp/archlinux/$repo/os/$arch",
    "rsync://jpn.mirror.pkgbuild.com/packages/$repo/os/$arch",
    "rsync://archlinux.cs.nctu.edu.tw/archlinux/$repo/os/$arch",
    "rsync://ftp.tku.edu.tw/archlinux/$repo/os/$arch",
    "rsync://hkg.mirror.rackspace.com/archlinux/$repo/os/$arch"]
```

上のように書き換える.

### pacman設定
このままではpowerpill実行のたびにエラーメッセージが表示されてしまう.[^powerpill_error]
脚注のサイトの通り, `/etc/pacman.conf` の　`SigLevel` の設定を

```conf
SigLevel = PackageRequired
```

に変更する.

### yay設定
pacmanの代わりにpowerpillを実行する場合は, コマンド引数として `--pacman powerpill` を与える必要がある.
毎度入力するのは面倒くさいため, エイリアスを設定しておくといいだろう.
[fish](https://fishshell.com/)の場合なら, `~/.config/fish/config.fish` に

```text
alias yay "yay --pacman powerpill"
```

と設定しておくと, いつも通りのコマンドで並列にダウンロードがなされる.

## 色付きの出力
リポジトリの`README.md`に書かれている通り, `/etc/pacman.conf`内にある

```conf
#Color
```

のコメントアウトを外し

```conf
Color
```

とする.

## 参考文献
* [Reflector - ArchWiki](https://wiki.archlinux.jp/index.php/Reflector)
* [Powerpill - ArchWiki](https://wiki.archlinux.jp/index.php/Powerpill)
* [AUR ヘルパー - ArchWiki](https://wiki.archlinux.jp/index.php/AUR_%E3%83%98%E3%83%AB%E3%83%91%E3%83%BC)
* [Jguer/yay: Yet another Yogurt - An AUR Helper written in Go](https://github.com/Jguer/yay)

[^parallel]: https://github.com/Jguer/yay/issues/685
[^powerpill_error]: https://wiki.archlinux.jp/index.php/Powerpill#.E3.83.88.E3.83.A9.E3.83.96.E3.83.AB.E3.82.B7.E3.83.A5.E3.83.BC.E3.83.86.E3.82.A3.E3.83.B3.E3.82.B0
