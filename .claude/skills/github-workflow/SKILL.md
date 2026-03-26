---
name: github-workflow
description: This skill should be used when the user asks to "GitHubに変更を反映", "PRを作成", "ブランチを作成", "push", "プルリクエストを発行", or any GitHub operation including committing, pushing, branching, or syncing changes in this project.
---

# GitHub ワークフロー（KanColle Extension）

## ブランチ運用

変更を GitHub へ反映する際は必ず `feature-` から始まるブランチを作成し、
変更をプッシュした後に `main` へのマージプルリクエストを発行する。

```
feature-<変更内容の概要>  →  PR  →  main
```

## Git および GitHub 操作は MCP 経由

Git（コミット等）および GitHub の操作（ブランチ作成・プッシュ・PR発行・コメント等）は
**必ず `mcp__github__*` ツールを経由して行う。**
`gh` コマンドや `git push` による直接操作は禁止。

## 手順

### 1. ブランチを作成してローカルで切り替える

リモートにブランチを作成し、ローカルも同じブランチに切り替える:

```
mcp__github__create_branch
  repo: <プロジェクト名>
  branch: feature-<変更内容の概要>
  from_branch: main
```

```bash
git fetch origin && git checkout -b feature-<変更内容の概要> origin/main
```

### 2. ファイルをプッシュする

```
mcp__github__push_files
  repo: <プロジェクト名>
  branch: feature-<変更内容の概要>
  files: [{ path: "<ファイルパス>", content: "<ファイル内容>" }]
  message: "<コミットメッセージ>"
```

`mcp__github__push_files` はローカル git を更新しないため、push 後にローカルの未コミット変更をスタッシュし、そのインデックスを変数に保持する:

```bash
git stash push -m "mcp-push: feature-<変更内容の概要>"
STASH_IDX=$(git stash list | grep "mcp-push: feature-<変更内容の概要>" | head -1 | grep -oP '(?<=stash@\{)\d+(?=\})')
```

### 3. プルリクエストを発行する

```
mcp__github__create_pull_request
  repo: <プロジェクト名>
  title: "<PR タイトル>"
  head: feature-<変更内容の概要>
  base: main
  body: "<PR 説明>"
```

### 4. マージ後のローカル同期

PR マージ後、main ブランチに切り替えて fast-forward し、手順2で保持したインデックスのスタッシュを削除する:

```bash
git checkout main && git pull --ff-only origin main
git stash drop stash@{$STASH_IDX}
```
