---
title: Git の歴史を映画のように再生する gitlogue を作りました
tags:
  - Git
  - Terminal
  - Rust
  - CLI
  - tui
private: false
updated_at: '2025-11-23T10:51:25+09:00'
id: 7e2d792dbf7993ac4465
organization_url_name: null
slide: false
ignorePublish: false
---

## はじめに

最近 [Omarchy](https://omarchy.org) を導入したり、[r/unixporn](https://www.reddit.com/r/unixporn/) を眺めていて、ターミナルスクリーンセーバーの存在を知りました。
[cbonsai](https://gitlab.com/jallbrit/cbonsai) や [asciiquarium](https://github.com/cmatsuoka/asciiquarium)、[tarts](https://github.com/oiwn/tarts) など、ターミナルを彩る様々なツールがあります。

そのうちなにか自分で作ってみたいと思い、以前 [GitType](https://github.com/unhappychoice/gittype) を作った経験から、 Git のコミットを題材にしたスクリーンセーバーを実装することにしました。

そうして完成したのが **gitlogue** です。
Git のコミット履歴をシネマティックなアニメーションとして再生する CLI ツールです。

![](https://raw.githubusercontent.com/unhappychoice/gitlogue/main/docs/assets/demo.gif)

## 特徴

* 🎬 **コミットをアニメーション再生**
* 🌳 **プロジェクトのファイルツリー**
* 🎨 **Tree-sitter によるシンタックスハイライト***
* 🖥️ **スクリーンセーバー**
* 🎭 **9 種類のテーマ**

## インストール

### ワンライナー (推奨)
```bash
curl -fsSL https://raw.githubusercontent.com/unhappychoice/gitlogue/main/install.sh | bash
```

### Homebrew (macOS/Linux)
```bash
brew install unhappychoice/tap/gitlogue
```

### Cargo
```bash
cargo install gitlogue
```

## 使い方

以下のコマンドで、スクリーンセーバーが起動します。

```bash
gitlogue
```

特定のコミットを表示したい場合は、コミットハッシュまたは範囲を指定します。

```bash
# 特定のコミットを再生
gitlogue --commit abc123

# 直近5コミットを再生
gitlogue --commit HEAD~5..HEAD

# 古い順に再生
gitlogue --order asc

# ループ再生
gitlogue --commit abc123 --loop
```

その他

```bash
# テーマを変更
gitlogue --theme dracula

# タイピング速度を調整 (文字あたりのミリ秒)
gitlogue --speed 20

# 特定のファイルパターンを無視
gitlogue --ignore "*.ipynb" --ignore "poetry.lock"

# テーマ一覧を表示
gitlogue theme list

# デフォルトテーマを設定
gitlogue theme set dracula
```

設定ファイル (`~/.config/gitlogue/config.toml`) でカスタマイズも可能です。
詳細は [Configuration Guide](https://github.com/unhappychoice/gitlogue/blob/main/docs/configuration.md) を参照してください。

## ユースケース

* 🖥️ **スクリーンセーバー** — ランダムなコミットを無限再生
* 📺 **プレゼンテーション** — 実際のコミット履歴をライブで再生
* 🎬 **コンテンツ制作** — VHS や asciinema でデモを録画
* 🎓 **学習用途** — コードの進化を視覚化し、学習材料として
* 🎨 **デスクトップカスタマイズ** — デスクトップの彩りに

## なぜ作ったか
以前作った GitType で、ratatui や tree-sitter といった TUI の技術には慣れていました。
せっかくなので、それを活かして「Git 版のスクリーンセーバー」を作ってみようと思い立ちました。

実装にあたっては、以下のような点を意識しました。

* **リアルなタイピング表現**
    * 単に diff を流すのではなく、実際にコードを書いているような動きを再現。
    * カーソル移動や削除の動きも含めて、「コミットの過程」を見せる。

* **視覚的な楽しさ**
    * ターミナルスクリーンセーバーとしても使えるよう、見た目にこだわりました。
    * テーマのカスタマイズやシンタックスハイライトで、ただ眺めているだけでも楽しめるように。

* **Git との自然な統合**
    * `git log` や `git diff` といった既存のワークフローを置き換えるのではなく、補完する形で。
    * 特定のコミットを指定したり、範囲を再生したりと、柔軟に使えるように。

## 技術

* Rust
* ratatui (TUI実装)
* tree-sitter (シンタックスハイライト、26言語対応)
* git2-rs (Git 操作)

GitType を作った経験が大いに役立ちました。
特に ratatui での TUI 実装や tree-sitter によるシンタックスハイライトの実装は、前回の知見をそのまま活かせました。
おかげで、UI のデザインやアニメーションの実装に集中できたのは大きかったです。

## 対応言語

Rust, TypeScript, JavaScript, Python, Go, Ruby, Swift, Kotlin, Java, PHP, C#, C, C++, Haskell, Dart, Scala, Clojure, Zig, Elixir, Erlang, HTML, CSS, JSON, Markdown, YAML, XML

## リンク

* GitHub: [https://github.com/unhappychoice/gitlogue](https://github.com/unhappychoice/gitlogue)
* Terminal Trove Tool of The Week: [https://terminaltrove.com/gitlogue/](https://terminaltrove.com/gitlogue/)

## 関連プロジェクト

* [**GitType**](https://github.com/unhappychoice/gittype) - 自分のコードでタイピング練習ができる CLI ゲーム

## おわりに

gitlogue は「ちょっと面白いかも」という軽い気持ちで作ったツールでした。

ところがリリースしてみると、[Terminal Trove](https://terminaltrove.com/gitlogue/) の Tool of The Week に選ばれたり、1200 star 以上をいただいたりと、思いがけない反響がありました。
正直なところ、そんな大層なツールだとは思っていなかったので、自分でも驚いています。

振り返ってみると、UI のデザインにこだわったのが良かったのかもしれません。
ただコミット履歴を表示するだけでなく、「見ていて楽しい」「ターミナルに置いておきたい」と思ってもらえるような見た目を目指しました。

スクリーンセーバーとして、プレゼンテーションツールとして、あるいは単に Git の履歴を眺める楽しみとして、ぜひ試してみてください。

```bash
gitlogue
```
