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
- `direnv`（任意。導入済みなら移動先の `.envrc` を読み込んで `claude` を起動する）

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

## direnv 連携

`direnv` が `PATH` に存在すると、`cl` は移動先で `direnv exec .` 越しに `claude` を起動する。移動先または祖先ディレクトリの `.envrc` に定義した環境変数が Claude Code セッションおよびスキルにそのまま継承される。

用途例:

- `ghq` の org ディレクトリ（例: `~/ghq/github.com/<org>/.envrc`）で `.envrc` を定義しておき、配下の全リポジトリで共通の環境変数を Claude Code に注入する。
- `.envrc` 内で `op read "op://..."` を呼び、1Password から秘密情報を取得して `claude` に渡す。

無効化するには `CL_NO_DIRENV` に非空の値を設定する。

```sh
CL_NO_DIRENV=1 cl
```

`.envrc` が未 allow 状態の場合は direnv が警告を出すが、`claude` の起動自体は続行する。初回のみ `direnv allow` の実行が必要。
