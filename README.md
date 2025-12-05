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

このプロジェクトでは、GitHub issue 管理などに **GitHub 公式のリモート MCP サーバー**を使用します。

このサーバーは HTTP ベースで動作するため、Claude Code on the Web でも利用できる可能性があります。

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

#### 3. MCP サーバーの設定内容

`.mcp.json` には以下の設定が含まれています：

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer ${GITHUB_PERSONAL_ACCESS_TOKEN}"
      }
    }
  }
}
```

この設定は GitHub が公式にホストしている HTTP ベースの MCP サーバーに接続します。

#### 4. 動作確認

環境変数を設定後、Claude Code を再起動すると、GitHub 関連の操作（issue 作成など）が利用可能になります。

**注意**:
- デスクトップ版の Claude Code では確実に動作します
- Claude Code on the Web での動作は環境により異なる場合があります

## ライセンス

MIT

## 履歴

- 2019-03-20: 初回リリース
- 2025-10-26: MCP サーバー設定追加
- 2025-10-27: HTTP ベースの GitHub 公式 MCP サーバーに移行
