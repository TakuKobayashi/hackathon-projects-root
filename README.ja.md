# hackathon-projects-root

[English README](./README.md)

hackathon-projects-root は、ハッカソンで開発するプロジェクトを追加・管理するための親リポジトリです。
`projects/` 配下に各プロジェクトを配置し、メタデータ管理・翻訳・一覧生成・Git Submodule 運用を共通のコマンドで行います。

---

## hackathon-projects-root の目的

- ハッカソンごとのプロジェクトを一元管理する
- 限られた開催時間内に動くデモを作る
- まず動く状態にして判断しやすくする
- 改善・再利用・統合しやすい形で蓄積する
- 有望なものは独立運用・公開へ進める

---

## 基本方針

### 1. まず“動く形”にする

生成AIを活用して、ゼロからすべてを手作業で作る時間を圧縮し、  
まず一定水準で可動する状態まで高速に到達することを優先します。

### 2. 調整・改善はその後

可動状態になっていれば、

- 必要な改善点の判断
- 他プロジェクトへの統合
- 他者への説明
- 公開判断
  が容易になります。

### 3. サンプルや技術検証も資産化

サンプル用途や学習目的のプロジェクトであっても、  
“ちゃんと動く” 状態で蓄積することで、
将来的な転用・統合・再利用コストを下げます。

---

## プロジェクトカテゴリ

### Product Candidate

公開・サービス化・収益化候補

### Utility / Automation

CLI / GitHub Actions / 自動化基盤

### Technical Asset

技術検証 / サンプル / 将来統合用

---

## 開発フェーズ

- incubating → アイディア具現化中
- validating → 改善・運用調整中
- launched → 公開 / 独立運用可能
- archived → 保守停止 / 技術資産化

---

## プロジェクト一覧

| プロジェクト                 | 説明 | status |
| ---------------------------- | ---- | ------ |
| プロジェクトはまだありません |      |        |

---

## 初期セットアップ

Node.js と pnpm を用意し、`pnpm install` を実行します。日本語の説明を自動翻訳する場合は Ollama を起動し、`ollama pull qwen3:4b` でモデルを取得してください。翻訳対象がなければ Ollama への接続は発生しません。

README の本文は `templates/README.template.md` と `templates/README.ja.template.md` を編集してください。同期時に両言語の README が再生成されます。新規プロジェクトは `projects/` に追加し、既存リポジトリは Git Submodule として登録できます。

## 開発フロー

### 新規プロジェクト追加

```bash
npm run projects:add -- --name my-new-project --description "Project description"
```

### portfolio / README同期

```bash
npm run projects:sync
```

### project.yml検証

```bash
npm run projects:validate
```

---

## Submodule運用（ローンチ後）

### 初回clone

```bash
git clone --recurse-submodules <repo-url>
```

### 他PCで最新取得（親 + 全submodule）

```bash
npm run projects:pull
```

### 開発内容を全反映

```bash
npm run projects:push
```

### 状態確認

```bash
npm run projects:status
```

---

## リポジトリ運用方針

このリポジトリでは、まず `projects/` 配下でプロジェクトを管理します。

ある程度形になり、独立したリポジトリとして運用したくなった場合は GitHub 上で別リポジトリとして切り出します。

その後、Git Submodule として再度取り込むことで、

- プロジェクトごとの独立運用
- リポジトリの一覧管理
- portfolio自動生成

を両立しています。

運用イメージ:

    projects/
    ├── project-a
    ├── project-b
    └── project-c

↓

    projects/
    ├── project-a (Submodule)
    ├── project-b (Submodule)
    └── project-c

### プロジェクトを独立リポジトリ化する

まず対象ディレクトリへ移動します。

    cd projects/my-project

GitHubで新規リポジトリを作成後、pushします。

    git init
    git add .
    git commit -m "Initial commit"

    git branch -M main

    git remote add origin https://github.com/<user>/my-project.git

    git push -u origin main

親リポジトリ側からディレクトリを削除します。

    git rm -r projects/my-project

    git commit -m "Remove local project"

その後 Git Submodule として再登録します。

    git submodule add https://github.com/<user>/my-project.git projects/my-project

    git commit -m "Add submodule"

### Submodule込みでcloneする

    git clone --recursive <repository-url>

### 後からSubmoduleを取得する

    git submodule update --init --recursive

### 全Submoduleを最新化する

    git submodule update --remote

### 誤って追加したSubmoduleを削除する

    git submodule deinit -f projects/my-project

    git rm -f projects/my-project

    rm -rf .git/modules/projects/my-project

---

## 最終ビジョン

hackathon-projects-root は、ハッカソンで生まれたプロトタイプ・デモ・技術検証を蓄積し、イベント終了後の改善・再利用・公開につなげるための開発基盤です。
