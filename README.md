# claude-code-launcher

ghq 配下のリポジトリを fzf で選び、そのディレクトリで `claude` を起動するランチャースクリプト。

## インストール

`bin/` を `PATH` に通す。

```sh
git clone https://github.com/mayaton/claude-code-launcher.git ~/ghq/github.com/mayaton/claude-code-launcher
export PATH="$HOME/ghq/github.com/mayaton/claude-code-launcher/bin:$PATH"
```

## 依存

- `ghq`
- `fzf`
- `claude`（Claude Code CLI）
- `herdr` と `jq`（herdr セッション内で使う場合）
- `zellij`（Zellij セッション内で使う場合）

## 使い方

```sh
cl [-- CLAUDE_ARGS...]
```

`--` 以降の引数は `claude` にそのまま渡す（herdr / 非マルチプレクサモード。Zellij モードは非対応）。

## 挙動

環境変数を見て、次のいずれかのモードで動作する。

- **herdr セッション内**（`$HERDR_PANE_ID` が定義済み）：リポ名の workspace があればフォーカスし、無ければ新規作成してその初期ペイン内で `claude` を起動する。
- **Zellij セッション内**（`$ZELLIJ` が定義済み）：リポ名のタブを新規作成し、その cwd で `claude` を起動する。
- **それ以外**：現在のシェルで `cd` してから `claude` に exec する。
