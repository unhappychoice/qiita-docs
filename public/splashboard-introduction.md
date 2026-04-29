---
title: シェル起動時と cd のたびにダッシュボードを描く Rust 製 CLI「splashboard」を作りました
tags:
  - Terminal
  - Rust
  - CLI
  - tui
  - ratatui
private: false
updated_at: '2026-04-29T14:42:01+09:00'
id: 3f7391738b0e36ffae40
organization_url_name: null
slide: false
ignorePublish: false
---

## はじめに

ブラウザで GitHub のレポジトリページを開くと、 README 、 commit 履歴、 Issue / PR 一覧、 contribution heatmap などがトップにまとまっていて、いま何が起きているかがぱっと見でわかります。

一方、ターミナルを開いたときに見えるのはプロンプト 1 行くらいで、出てくる情報量はかなり少なめです。

**ターミナルでも Web / GitHub くらいの情報量を出せないか**、と思って作ったのが今回紹介する **splashboard** です。
シェル起動時と `cd` のたびにカスタマイズ可能なダッシュボードを描画する Rust 製の CLI ツールです。

![splashboard.gif](https://github.com/unhappychoice/splashboard/blob/main/docs/screenshots/demo.gif?raw=true)

![splashboard.png](https://github.com/unhappychoice/splashboard/blob/main/docs/screenshots/project_github.png?raw=true)

## 特徴

* 🌊 **シェル起動時 / `cd` のたびに描画**  
  * `.zshrc` に `eval "$(splashboard init zsh)"` を 1 行入れるだけ。
* 🎨 **TOML で自在にカスタマイズ**  
  * widget / 配置 / 見た目を [`dashboard.toml`](https://splashboard.unhappychoice.com/guides/configuration/) で自由に組み立て可能。情報を詰め込むのも、極限までシンプルにするのも好みで。
* ✨ **アニメーション系から情報密度系まで揃う renderer**  
  * ASCII アート、 scanline / 波 / スプリットフラップなどリッチな見た目から、 heatmap / sparkline / timeline / calendar / chart などダッシュボードらしく情報を詰める表示まで対応（[renderer 一覧](https://splashboard.unhappychoice.com/reference/matrix/)）。
* 📦 **Home / Project のデフォルトとプリセット同梱**  
  * `splashboard install` のライブプレビューから、 home / project それぞれの[プリセット dashboard](https://splashboard.unhappychoice.com/guides/presets/) と[テーマ](https://splashboard.unhappychoice.com/guides/themes/)（ `tokyo_night` / `catppuccin_mocha` / `dracula` / `nord` / `gruvbox_dark` など）を選ぶだけで使える。
* 📁 **レポジトリごとに上書き可能**  
  * レポジトリ直下に `./.splashboard/dashboard.toml` を置けば、 `cd` した瞬間にそのレポジトリ専用の splash に切り替わる。
* ⚡ **シェル起動時の描画が速い**  
  * 初回はキャッシュ済みのデータから即座に描画されるので、シェル起動を待たせない。最新データはバックグラウンドで再取得。
* 🔒 **per-directory config の [trust モデル](https://splashboard.unhappychoice.com/guides/trust/)**  
  * クローンしたレポジトリ同梱の `dashboard.toml` で URL を config から渡す系の widget （ rss / calendar など）は、 `splashboard trust` を通すまで動かない。

## インストール

### ワンライナー（ Linux / macOS / Windows ）
```
curl -fsSL https://raw.githubusercontent.com/unhappychoice/splashboard/main/install.sh | bash
```

### Homebrew （ Linux / macOS ）
```
brew install unhappychoice/tap/splashboard
```

### ソースからビルド Cargo （ Universal ）
```
cargo install splashboard
```

## 使い方

インストール後、対話セットアップを起動します。

```bash
splashboard install
```

シェル（ zsh / bash / fish / PowerShell ）を自動検出し、 Home 用 dashboard / Project 用 dashboard / テーマをライブプレビュー付きで選ぶと、 `~/.splashboard/` 以下に設定が書かれて rc に init スニペットが追記されます。

新しいシェルを開けば、もうそこに splash が出ます。

レポジトリに同梱したい場合は、レポジトリ直下に `.splashboard/dashboard.toml` を置きます。

```toml
[[widget]]
id = "branch"
fetcher = "git_branch"
render = "text_plain"

[[widget]]
id = "ci"
fetcher = "github_ci_status"
render = "status_badge"

[[row]]
height = { length = 1 }
  [[row.child]]
  widget = "branch"
  [[row.child]]
  widget = "ci"
```

`cd` した瞬間にこの dashboard に切り替わります。

## なぜ作ったか

ターミナル起動時に何かを表示するツール自体はいくつかありますが、 GitHub のレポジトリページくらいの情報密度を出すものは見当たりませんでした。

* **system info ツール（ neofetch / fastfetch ）**  
  OS / カーネル / メモリの表示が中心で、レポジトリの状態のような動的なデータは出てこない。
* **prompt （ starship ）**  
  1 行のスペースに集約する道具。 ASCII アート、 heatmap 、 timeline のような大きな表示はできない。
* **MOTD ( `/etc/motd` 、 `update-motd` ) などのログインメッセージ**  
  SSH ログイン時の伝統的なテキスト表示機能。 update-motd で動的にもできるが、サーバ管理者目線の機能でレポジトリ単位の cd 連動やリッチな TUI 表示はできない。
* **shell rc に書いた手製のスクリプト**  
  表示したい情報が増えると、すぐに管理しづらくなる。

シェル起動時や `cd` のたびに GitHub のレポジトリページくらいの情報量を出してくれるツールがほしくて、 splashboard を作りました。

## 技術

* Rust
* [ratatui](https://ratatui.rs/) （ TUI 描画）
* [crossterm](https://github.com/crossterm-rs/crossterm) （端末制御）
* [tokio](https://tokio.rs/) （バックグラウンドのキャッシュリフレッシュ）
* [git2](https://github.com/rust-lang/git2-rs) （ git 系 fetcher ）
* Claude Code をヘビーユース

## リンク

https://github.com/unhappychoice/splashboard

* ドキュメントサイト: [https://splashboard.unhappychoice.com/](https://splashboard.unhappychoice.com/)
  * [プリセット一覧](https://splashboard.unhappychoice.com/guides/presets/) : home / project プリセットの実レンダリング結果が並ぶ。
  * [テーマ一覧](https://splashboard.unhappychoice.com/guides/themes/) : 同梱のカラーパレット一覧。
  * [showcase](https://splashboard.unhappychoice.com/showcases/) : 実出力をそのまま埋め込んだ splashboard 紹介ページ。
  * [リファレンス](https://splashboard.unhappychoice.com/reference/matrix/) : 全 fetcher / renderer の一覧と組み合わせ表。
* crates.io: [https://crates.io/crates/splashboard](https://crates.io/crates/splashboard)

## おわりに

GitHub のレポジトリページを開かなくても、ターミナルだけでレポジトリの現在地がだいたいわかる、くらいを目指しています。
気になった方はぜひ試してみてください。

```bash
curl -fsSL https://raw.githubusercontent.com/unhappychoice/splashboard/main/install.sh | bash
splashboard install
```
