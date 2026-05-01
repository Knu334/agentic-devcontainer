# Claude Code DevContainer

Claude Codeプロジェクト向けDevContainerテンプレート。  
デフォルトで[Serena MCP](https://github.com/oraios/serena)（トークン消費改善用）をHTTPモードでセットアップする。

## 前提要件

- [Docker](https://www.docker.com/ja-jp/) がインストール済みであること
- VS Code に [Dev Container拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)がインストール済みであること

## 要修正箇所

- **CLAUDE.md** — プロジェクト概要を記載する必要あり。
- **.devcontainer/.env.sample** — .envにコピーの上、`GH_TOKEN`・`GIT_USER_EMAIL`・`GIT_USER_NAME`・`PJ_NAME` を設定する必要あり（`CONTEXT7_API_KEY` はContext7を利用する場合に設定）。
- **.devcontainer/init-firewall.sh** — Claude Codeのアクセス制限のため指定ドメイン以外へのアクセスを拒否している。  
許可ドメインを追加する場合は`# Resolve and add other allowed domains`セクションにドメイン追加する。  
デフォルトでは`Github` `Google` `Anthropic API` `VSCode関連` `NPMレジストリ` `Context7`を許可。
- **.devcontainer/devcontainer.json** — DevContainer内で使用したいVS Code拡張機能がある場合は`customizations.vscode.extensions`に拡張機能の識別子を追加する。

## 使用方法

1. Github WebUIから新規リポジトリを作成する。
2. 本リポジトリをベアクローンする。
```bash
git clone --bare https://github.com/Knu334/ClaudeCodeDevContainer.git
```
3. クローンしたディレクトリに移動する。
```bash
cd ClaudeCodeDevContainer.git
```
4. 新規リポジトリにテンプレート内容を反映する。
```bash
git push --mirror <新規リポジトリのGithubリンク>
```
5. ローカルのテンプレートリポジトリディレクトリを削除する。
```bash
cd ..
rm -rf ClaudeCodeDevContainer.git
```
6. 新規リポジトリをクローンする。
```bash
git clone <新規リポジトリのGithubリンク>
```
7. VS Codeでローカルリポジトリを開く。
8. コンテナーで現在のフォルダを開く を実行する。
9. ターミナルを開くとClaude Codeが使用可能となる。
10. Serena MCPのセットアップをClaude Codeに実行させる。
```
mcp__serena__onboardingを実行して
```

## テンプレートの更新取り込み方法

1. テンプレートを導入したローカルリポジトリに移動する。
2. テンプレートリポジトリをリモートに追加する。
```bash
git remote add template https://github.com/Knu334/ClaudeCodeDevContainer.git
```
3. フェッチする。
```bash
git fetch --all
```
4. テンプレートの変更をマージする。
```
git merge template/main
```
5. 競合が発生した場合は解消する。
6. リモートリポジトリにプッシュする。
```bash
git push
```
