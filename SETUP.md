# 環境構築ガイド (SETUP.md)

このドキュメントは、人間とAI（Gemini）が協調してローカル開発環境を構築するための手順書です。

## Phase 1: 初期セットアップ（Web版Geminiを使用）

以下のプロンプトをコピーしてWeb版Geminiに貼り付けてください。

### 【Web版Geminiへの入力プロンプト】

```markdown
# 厳守事項（同期実行・ノイズ排除ルール）

あなたは「一問一答形式」の環境構築ガイドです。以下のルールを1文字たりとも違えず守ってください。

1. **ノイズの完全排除**: 「Gemini アプリ アクティビティ」に関する言及、および履歴設定などのWeb版独自の仕様は、今回のCLIセットアップには一切関係ありません。これらの文言は絶対に出力しないでください。
2. **シングルタスク**: 1つの返信につき、要求するアクションは「1つだけ」です。「あわせてこれも実行してください」といったまとめ要求は禁止です。
3. **先読み禁止**: 現在のステップが完了するまで、次のステップの手順や概念を一切口にしないでください。
4. **捏造禁止**: `gemini login` は存在しません。認証は引数なしの `gemini` を実行し、メニュー選択する仕様を案内すること。

# 1. ターゲットバージョンの特定（検索必須）

まず、以下のツールの「2026年現在の最新安定版」を自ら検索・特定してください。

* **Node.js**: 最新のLTS（長期サポート）バージョン
* **Git for Windows**: 最新の Stable リリース
* **GitHub CLI (gh)**: 最新の Stable リリース
* **Gemini CLI (@google/gemini-cli)**: 最新バージョン

# 2. 認証仕様（公式基準）

* **個人のGoogleアカウント**: Googleでログイン / `GOOGLE_CLOUD_PROJECT` 設定不要。
* **組織（Workspace等）ユーザー**: Googleでログイン / `GOOGLE_CLOUD_PROJECT` が必須。`~/.gemini/.env` に記述。
* **AI Studio ユーザー**: Gemini APIキーを使用 / プロジェクト設定不要。
* **Vertex AI ユーザー**: Vertex AI / Project と Location の両方が必須。

# 3. 認証と疎通確認の仕様

1. 引数なしの `gemini` で認証メニューを通る。
2. 完了後、そのまま始まる対話モードで「おはよう」と入力して動作確認する。
3. 成功後、シェルに戻るための終了手順を案内する。
   * `/quit` と入力する
   * または、`Ctrl+C` を 2回 入力する

# 4. セットアップ順序

1. 検索による「各ツールの最新安定バージョン」の特定と提示（ここで一旦停止）。
2. `node -v`, `git --version`, `gh --version`, `gemini --version` の確認と判定。
3. `gh auth status` によるGitHub認証および Git 連携状態（`git config --get-all credential.helper`）の確認。
4. 未導入ツールのインストール、および必要に応じた 認証の案内。
5. ユーザータイプの質問と、タイプに応じた認証・環境設定の案内。
6. 対話モードでの疎通確認（「おはよう」）と、終了手順の案内。

# 最初のタスク

あなたが特定した「最新安定バージョンの数値」を提示し、私に「`node -v`, `git --version`, `gh --version`, `gemini --version` を実行して報告してください」とだけ伝えてください。
提示後、私の返答を待たずに次の確認（GitHub認証など）を求めないでください。ここで一旦止まってください。
```

---

## Phase 2: 環境の完成（Gemini CLIを使用）

Gemini CLIが動くようになったら、以下を実行します。

前提：SETUP.mdがあるリポジトリをクローンしてください

```bash
# シェルに戻った状態で実行
gemini "SETUP.md を読み、「ゴール（目指す状態）」を確認してください。現状を確認して未完了の項目があれば解決策を1つずつ提示してください。"
```

---

## 🎯 ゴール（目指す状態）

### シェル環境

* Windowsなら Git Bash、Macなら Bash/Zsh が使えること

```bash
echo $0
```

### Node.js

* 最新安定版LTSがインストールされていること

```bash
node -v
```

### GitHub CLI

* gh コマンド(最新安定版)がインストールされていること
* gh でログイン済であること
* gh でGitHubリポジトリ参照ができること

```bash
gh --version
gh auth status
gh repo view
```

### Git

* git コマンド(最新安定版)インストール済であること
* git と gh の認証連携ができていること
* git でGitHubリポジトリ参照ができること
* git でのコミット準備ができていること（`user.name`、`user.email`）

```bash
git --version
git config --get-all credential.helper
git status
git branch
git config user.name
git config user.email
```

### Docker

* デーモンが起動し、応答すること (最新安定版)

```bash
docker --version
docker ps
```

### gcloud CLI

* 最新安定版インストール済であること
* OAuth認証済であること
* ADC認証済であること

```bash
gcloud version 
gcloud auth list
gcloud auth application-default print-access-token
```

### Terraform

* 最新安定版インストール済であること

```bash
terraform version
```

### tflint

* 最新安定版インストール済であること

```bash
tflint --version 
```

### markdownlint-cli

* 最新安定版インストール済であること
* 行桁数制限（MD013）は無効になっていること（VSCode拡張のデフォルトに合わせるため）

```bash
markdownlint --version
cat .markdownlint.json
```

### VSCode

* 最新安定版インストール済であること

```bash
code --version
```

### VSCode拡張

* 以下が導入されていること
  * HashiCorp Terraform VSCode extension `hashicorp.terraform`
  * Cloud Code for VS Code 拡張機能 `googlecloudtools.cloudcode`
  * markdownlint `davidanson.vscode-markdownlint`

```bash
code --list-extensions
```

### Gemini CLI

* 最新安定版インストール済であること
* 認証済であること

```bash
gemini --version
```

### Gemini CLI拡張機能

* `terraform-mcp-server` が導入されていること
  * [hashicorp/terraform\-mcp\-server: The Terraform MCP Server provides seamless integration with Terraform ecosystem, enabling advanced automation and interaction capabilities for Infrastructure as Code \(IaC\) development\.](https://github.com/hashicorp/terraform-mcp-server?tab=readme-ov-file#usage-with-gemini-extensions)

```bash
gemini --extensions
```

* **Gemini MCP**: 以下のサーバーが稼働していること
  * **Developer Knowledge MCP server**
    * [Connect to the Developer Knowledge MCP server  \|  Google for Developers](https://developers.google.com/knowledge/mcp)

```bash
gemini mcp list
```

---
