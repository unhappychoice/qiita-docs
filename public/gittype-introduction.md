---
title: 自分のコードで練習ができるCLIタイピングゲームの GitType を作りました
tags:
  - Git
  - game
  - Rust
  - CLI
  - 初心者
private: false
updated_at: '2025-09-08T21:05:59+09:00'
id: e76099d2ecd0cf91fb47
organization_url_name: null
slide: false
ignorePublish: false
---

## はじめに

タイピング練習ツールは昔から色々ありますが、出てくるのはサンプルコードや英単語が多いです。

それを速く打ててもあまり意味がなく、日常的に触っているコードで練習できれば便利だと思いました。

そこで今回 **GitType** を作りました。  
自分のコードや任意の OSS リポジトリを対象にタイピング練習ができる CLI ツールです。

![demo.gif](https://github.com/unhappychoice/gittype/blob/main/docs/images/demo.gif?raw=true)

## 特徴

* ⌨️ **実際のコードで練習**  
  * Web サービスのようにアップロードは不要。ローカルのリポジトリをそのまま利用可能。
* 📊 **履歴や分析が残る**  
  * ゲーム内の画面でスコアを振り返り可能。
* 🌍 **どんな OSS でも教材に**  
  * GitHub リポジトリを指定すれば、そのままタイピング対象に。
  * 新しい言語やプロジェクトに触れるきっかけにも。
* 🖥️ **TUI に慣れる**  
  * 最近は AI エージェントも CLI での利用が増えていますが、ターミナル操作の練習にもおすすめ。
* 🔗 **SNS 共有**  
  * スコアを共有して承認欲求チャージ。
* 🤖 **AI がコード生成している間の暇つぶし**
  * タイピング練習で、待ち時間も有益に（？）。
* 🌐 **多数のプログラミング言語に対応**  
  * [対応言語一覧](https://github.com/unhappychoice/gittype/blob/main/docs/supported-languages.md)

## インストール
### ワンライナー (Linux/macOS/Windows)
```
curl -sSL https://raw.githubusercontent.com/unhappychoice/gittype/main/install.sh | bash
```

### Homebrew (Linux/macOS)
```
brew install unhappychoice/tap/gittype
```

### ソースからビルド Cargo (Universal)
```
cargo install gittype
```

## 使い方

カレントディレクトリで現在のレポジトリを対象に起動できます。

```bash
gittype
```

特定のディレクトリを指定するなら、

```bash
gittype path/to/src
```

GitHub にある OSS リポジトリを直接指定/クローンすることもできます。（が、現状大きなリポジトリは起動に時間がかかることがあります）

```bash
gittype --repo git/git
gittype --repo https://github.com/rails/rails
gittype --repo git@github.com:axios/axios.git
```

## なぜ作ったか

既存のタイピングツールやサービスには以下のような不満がありました。

* **サンプルコードや英単語中心**  
  実際の開発に直結しづらい。

* **Web サービス型**  
  コードのアップロードが前提で、セキュリティ的にも面倒。

* **OSS を教材にできない**  
  新しい言語やフレームワークを触る練習に使えない。

こうした点を解消したくて GitType を作りました。

## 技術

* Rust
* ratatui（TUI実装。一部まだ crossterm での直実装）
* tree-sitter (ASTの利用、妥当なコード片の抽出に)
* Claude Code をヘビーユース

## リンク

* GitHub: [https://github.com/unhappychoice/gittype](https://github.com/unhappychoice/gittype)

## おわりに

GitType は自分のコードやOSSを叩いて遊べるタイピングゲームです。  
気になった方はぜひ試してみてください。
ちなみに作者は1万点ちょっとくらいが限界なので、タイピング得意な我こそはという方はぜひスコアを教えていただけると 🙏

```bash
gittype
```
