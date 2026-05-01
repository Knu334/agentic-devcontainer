## プロジェクト概要

<プロジェクトに合わせて修正する>

---

## Claude の作業ルール

- `/workspace` 配下のファイルを**参照**する際は、必ず **serena ツール**（`mcp__serena__*`）を利用する
- **編集**は `Edit` / `Write` ツールを基本とし、シンボル単位の大きな書き換えは serena ツールも活用する
- `node_modules` `dist` 配下は原則参照しない。参照が必要な場合はユーザーに許可を得てから行う

---

## GitHub ワークフロー

- 必ず `feature-<概要>` ブランチを作成し PR を発行する
- Git/GitHub 操作は **`gh` CLI** を使用する（`git push --force` / `git reset --hard` 禁止）

詳細手順: `/github-workflow` スキル参照
