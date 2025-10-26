# ちり毛神経衰弱

ランダム生成されたSVGパターンを使った神経衰弱（メモリーゲーム）

## 遊び方

ブラウザで `index.html` を開くとゲームが始まります。

### ゲーム設定

URLクエリパラメータで難易度を調整できます：

- `?level=4` - パターンの複雑さ（1〜4、デフォルト: 4）
- `?num_of_cards=10` - カードのペア数（デフォルト: 10ペア = 20枚）

**例:**
- `index.html?level=1&num_of_cards=5` - 簡単（10枚）
- `index.html?level=4&num_of_cards=15` - 難しい（30枚）

## 開発環境のセットアップ

### GitHub MCP サーバーの設定

このプロジェクトでは、GitHub issue 管理などに GitHub MCP サーバーを使用します。

#### 1. GitHub Personal Access Token の作成

1. https://github.com/settings/tokens にアクセス
2. "Generate new token (classic)" をクリック
3. 以下のスコープを選択：
   - `repo` (プライベートリポジトリの場合)
   - `public_repo` (パブリックリポジトリのみの場合)
4. トークンを生成してコピー

#### 2. 環境変数の設定

```bash
# .env.example をコピー
cp .env.example .env

# .env ファイルを編集してトークンを設定
# GITHUB_PERSONAL_ACCESS_TOKEN=your_actual_token_here
```

#### 3. MCP サーバーの有効化

Claude Code で MCP サーバーを有効にします：

```bash
# プロジェクトディレクトリで以下を実行
claude mcp add github --scope project
```

または、`.mcp.json` ファイルが自動的に読み込まれます。

#### 4. 動作確認

Claude Code を再起動後、GitHub 関連の操作（issue 作成など）が利用可能になります。

## ライセンス

MIT

## 履歴

- 2019-03-20: 初回リリース
- 2025-10-26: MCP サーバー設定追加
