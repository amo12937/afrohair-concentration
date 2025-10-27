# モダン化のためのIssue一覧

このドキュメントは、「ちり毛神経衰弱」アプリケーションをモダンなアーキテクチャに移行するための21個のissueを定義しています。

---

## Phase 1: CI/CD基盤構築

### Issue #1: GitHub Actions CI/CD パイプラインのセットアップ

**タイトル:** GitHub Actions CI/CD パイプラインのセットアップ

**背景:**
現在、アプリケーションのデプロイは手動で行われており、自動化されていません。モダンなアプリケーション開発では、継続的デプロイメント（CD）が標準となっています。GitHub Pagesを利用して、mainブランチへのマージ時に自動的にデプロイされる仕組みを構築する必要があります。

**作業内容:**
- `.github/workflows/deploy.yml` ファイルを作成
- 既存の `index.html` をそのまま GitHub Pages にデプロイする設定
- デプロイ成功後、`https://amo12937.github.io/afrohair-concentration/` でアクセス可能であることを確認

**クローズ条件:**
- [ ] `.github/workflows/deploy.yml` が作成されている
- [ ] masterブランチへのマージ時に自動デプロイが実行される
- [ ] `https://amo12937.github.io/afrohair-concentration/` で既存アプリが正常に動作する
- [ ] GitHub ActionsのワークフローがGreen（成功）状態である

**依存関係:** なし

**推定作業時間:** 1-2時間

---

## Phase 2: 開発環境構築（既存アプリ維持）

### Issue #2: Node.js プロジェクト初期化と Vite セットアップ

**タイトル:** Node.js プロジェクト初期化と Vite セットアップ

**背景:**
現在のアプリケーションは単一のHTMLファイルで構成されており、ビルドツールが存在しません。モダンな開発環境では、Viteなどのビルドツールを使用することで、開発効率の向上、ホットリロード、最適化されたビルド出力などの恩恵を受けられます。この段階では既存アプリの機能を維持しながら、Viteを導入します。

**作業内容:**
- `package.json` を作成（`npm init`）
- Vite をインストール（`npm install -D vite`）
- `vite.config.ts` を作成し、GitHub Pages用のベースパス設定（`base: '/afrohair-concentration/'`）
- 既存の `index.html` を `public/index.html` に配置するか、Viteのエントリーポイントとして設定
- `package.json` にスクリプト追加（`dev`, `build`, `preview`）
- GitHub Actions のデプロイワークフローをViteビルド経由に変更

**クローズ条件:**
- [ ] `package.json` が作成され、必要な依存関係が記載されている
- [ ] `vite.config.ts` が作成され、適切な設定がされている
- [ ] `npm run dev` でローカル開発サーバーが起動し、既存アプリが動作する
- [ ] `npm run build` でビルドが成功する
- [ ] GitHub Pages でViteビルド版のアプリが正常に動作する
- [ ] 既存アプリと完全に同じ機能・見た目である（機能退行なし）

**依存関係:** Issue #1

**推定作業時間:** 2-3時間

---

### Issue #3: TypeScript 環境のセットアップ

**タイトル:** TypeScript 環境のセットアップ

**背景:**
現在のコードはJavaScriptで書かれており、型安全性がありません。TypeScriptを導入することで、コンパイル時に型エラーを検出でき、開発効率とコード品質が向上します。この段階ではTypeScript環境のみを整備し、既存のJSコードはそのまま維持します。

**作業内容:**
- `typescript` をインストール（`npm install -D typescript`）
- `tsconfig.json` を作成（適切なコンパイラオプション設定）
- Viteの設定でTypeScriptサポートを有効化
- `package.json` に型チェックスクリプトを追加（`"typecheck": "tsc --noEmit"`）

**クローズ条件:**
- [ ] `typescript` がインストールされている
- [ ] `tsconfig.json` が作成され、適切な設定がされている
- [ ] `npm run typecheck` でTypeScriptの型チェックが実行できる（エラーなし）
- [ ] Viteビルドが引き続き成功する
- [ ] 既存アプリが引き続き正常に動作する

**依存関係:** Issue #2

**推定作業時間:** 1-2時間

---

## Phase 3: コード品質ツール

### Issue #4: ESLint + Prettier セットアップ

**タイトル:** ESLint + Prettier セットアップ

**背景:**
コード品質を保つためには、リンターとフォーマッターの導入が不可欠です。ESLintでコードの問題を検出し、Prettierで統一されたコードスタイルを維持することで、保守性の高いコードベースを構築できます。また、CI/CDパイプラインに統合することで、品質チェックを自動化します。

**作業内容:**
- `eslint`, `prettier`, 関連プラグインをインストール
- `eslint.config.js` を作成（Flat Config形式、TypeScript対応）
- `.prettierrc` を作成
- `.prettierignore` を作成
- `package.json` にスクリプト追加（`lint`, `format`）
- `.github/workflows/lint.yml` を作成（CIでlintチェック）
- 既存コードに自動フォーマット適用（動作変更なし）

**クローズ条件:**
- [ ] ESLint, Prettier がインストールされ、設定ファイルが作成されている
- [ ] `npm run lint` でlintチェックが実行できる
- [ ] `npm run format` でコード整形が実行できる
- [ ] GitHub ActionsでPR時に自動的にlintチェックが実行される
- [ ] 既存コードがフォーマットされているが、動作は変わらない

**依存関係:** Issue #3

**推定作業時間:** 2-3時間

---

## Phase 4: テスト環境構築

### Issue #5: Vitest テストフレームワークのセットアップ

**タイトル:** Vitest テストフレームワークのセットアップ

**背景:**
現在、テストが全く存在しないため、リファクタリングや機能追加時に回帰バグが発生するリスクがあります。Vitestを導入することで、高速なユニットテストを実行でき、コードの品質と信頼性を向上させることができます。

**作業内容:**
- `vitest` をインストール（`npm install -D vitest`）
- `vitest.config.ts` を作成
- `package.json` にテストスクリプト追加（`test`, `test:watch`, `test:coverage`）
- `.github/workflows/test.yml` を作成（CIでテスト実行）
- ダミーテストファイル作成（`src/dummy.test.ts`）で動作確認

**クローズ条件:**
- [ ] Vitest がインストールされ、設定ファイルが作成されている
- [ ] `npm run test` でテストが実行できる
- [ ] `npm run test:coverage` でカバレッジレポートが生成される
- [ ] GitHub ActionsでPR時に自動的にテストが実行される
- [ ] ダミーテストが成功する
- [ ] 既存アプリが引き続き正常に動作する

**依存関係:** Issue #3

**推定作業時間:** 2-3時間

---

## Phase 5: Infrastructure層の実装（独立したユーティリティ）

### Issue #6: シャッフルアルゴリズムの改善とテスト

**タイトル:** シャッフルアルゴリズムの改善とテスト

**背景:**
現在のシャッフルアルゴリズムは不完全なFisher-Yates実装であり、分布にバイアスがあります。正しいFisher-Yatesアルゴリズムを実装し、統計的なテストで均一性を検証する必要があります。この段階では独立したモジュールとして実装し、既存コードでは使用しません。

**作業内容:**
- `src/infrastructure/utils/shuffle.ts` を作成
- 正しいFisher-Yatesアルゴリズムを実装
- `src/infrastructure/utils/shuffle.test.ts` を作成
  - 配列長が保持されることのテスト
  - すべての要素が保持されることのテスト
  - 分布の均一性テスト（統計的検証）
- JSDocコメントを追加

**クローズ条件:**
- [ ] `shuffle.ts` が作成され、正しいFisher-Yatesアルゴリズムが実装されている
- [ ] `shuffle.test.ts` が作成され、すべてのテストが成功する
- [ ] テストカバレッジが100%である
- [ ] 既存アプリは旧実装を使用し続け、正常に動作する（まだ統合しない）

**依存関係:** Issue #5

**推定作業時間:** 2-3時間

---

### Issue #7: SVG パターン生成ロジックの抽出とテスト

**タイトル:** SVG パターン生成ロジックの抽出とテスト

**背景:**
現在のSVG生成ロジック（`ri`, `f`, `C`, `M`, `d` 関数）はグローバル変数として定義されており、テストや再利用が困難です。これらをクラス化し、独立したモジュールとして切り出すことで、テスト可能性と保守性が向上します。

**作業内容:**
- `src/infrastructure/svg/PatternGenerator.ts` を作成
- 既存のSVG生成ロジックをクラス化（`PatternGenerator` クラス）
- `src/infrastructure/svg/PatternGenerator.test.ts` を作成
  - 生成されるSVGパスの形式検証
  - レベルによる複雑さの検証
  - 乱数シードを使った再現性テスト
- JSDocコメントを追加

**クローズ条件:**
- [ ] `PatternGenerator.ts` が作成され、SVG生成ロジックがクラス化されている
- [ ] `PatternGenerator.test.ts` が作成され、すべてのテストが成功する
- [ ] テストカバレッジが90%以上である
- [ ] 既存アプリは旧実装を使用し続け、正常に動作する（まだ統合しない）

**依存関係:** Issue #5

**推定作業時間:** 3-4時間

---

## Phase 6: Domain層の実装（ビジネスロジック）

### Issue #8: Card エンティティとユースケースの実装

**タイトル:** Card エンティティとユースケースの実装

**背景:**
現在の`Card`クラスはDOMへの直接参照（`$dom`プロパティ）を持ち、UIとロジックが密結合しています。クリーンアーキテクチャでは、ドメイン層はUIに依存してはいけません。UI非依存の`Card`エンティティを実装することで、テスト可能性と保守性が向上します。

**作業内容:**
- `src/domain/entities/CardStatus.ts` を作成（enum: CONCEALED, REVEALED, CAPTURED）
- `src/domain/entities/Card.ts` を作成（UI非依存）
  - プロパティ: `id`, `pattern`, `status`
  - メソッド: `reveal()`, `conceal()`, `capture()`
- `src/domain/entities/Card.test.ts` を作成
  - 各メソッドの振る舞いテスト
  - 状態遷移の検証
- TypeScriptの型を活用

**クローズ条件:**
- [ ] `Card.ts` が作成され、UI非依存なエンティティとして実装されている
- [ ] `Card.test.ts` が作成され、すべてのテストが成功する
- [ ] テストカバレッジが100%である
- [ ] 既存アプリは旧実装を使用し続け、正常に動作する（まだ統合しない）

**依存関係:** Issue #5

**推定作業時間:** 3-4時間

---

### Issue #9: Game エンティティとユースケースの実装

**タイトル:** Game エンティティとユースケースの実装

**背景:**
現在の`Game`クラスもUIロジック（`render`, `onclick`）を含んでおり、ビジネスロジックとUIが混在しています。ゲームロジックをUI非依存なエンティティとして切り出すことで、テスト可能性と再利用性が向上します。

**作業内容:**
- `src/domain/entities/Game.ts` を作成（UI非依存）
  - プロパティ: `cards`, `firstSelectedIndex`, `isProcessing`
  - メソッド: `flipCard(index)`, `checkMatch()`, `isCompleted()`
- `src/domain/usecases/FlipCard.ts` を作成
- `src/domain/usecases/CheckMatch.ts` を作成
- `src/domain/entities/Game.test.ts` を作成
  - カードフリップのロジック検証
  - マッチング判定の検証
  - ゲーム完了判定の検証
- ユースケースのテストも作成

**クローズ条件:**
- [ ] `Game.ts` が作成され、UI非依存なエンティティとして実装されている
- [ ] ユースケース（`FlipCard.ts`, `CheckMatch.ts`）が作成されている
- [ ] すべてのテストが作成され、成功する
- [ ] テストカバレッジが100%である
- [ ] 既存アプリは旧実装を使用し続け、正常に動作する（まだ統合しない）

**依存関係:** Issue #8

**推定作業時間:** 4-5時間

---

## Phase 7: React環境のセットアップ

### Issue #10: React + TypeScript 環境構築

**タイトル:** React + TypeScript 環境構築

**背景:**
Vanilla JavaScriptから、コンポーネントベースのReactへ移行することで、UIの保守性と再利用性が向上します。この段階では、Reactの基本環境を構築し、既存アプリと並行して動作させることで、段階的な移行を実現します。

**作業内容:**
- `react`, `react-dom` をインストール
- `@vitejs/plugin-react` をインストール・設定
- `src/main.tsx` を作成（Reactエントリーポイント）
- `src/App.tsx` を作成（Hello World表示）
- `index.html` をReactエントリーポイントに変更
- 既存のゲームHTMLを `public/legacy.html` として保存
- GitHub Pagesで両方アクセス可能にする

**クローズ条件:**
- [ ] React環境がセットアップされている
- [ ] `/` にアクセスするとReactアプリ（Hello World）が表示される
- [ ] `/legacy.html` にアクセスすると既存アプリが表示される
- [ ] 両方のアプリがGitHub Pagesで正常に動作する

**依存関係:** Issue #9

**推定作業時間:** 2-3時間

---

### Issue #11: React Testing Library セットアップ

**タイトル:** React Testing Library セットアップ

**背景:**
Reactコンポーネントをテストするためには、React Testing Libraryが必要です。ユーザーの視点でコンポーネントをテストすることで、実際の使用シナリオに近いテストが可能になります。

**作業内容:**
- `@testing-library/react` をインストール
- `@testing-library/jest-dom` をインストール
- `@testing-library/user-event` をインストール
- `tests/setup.ts` を作成（テストセットアップファイル）
- `vitest.config.ts` を更新（React Testing Library統合）
- `src/App.test.tsx` を作成（サンプルテスト）
- CIでReactテストが実行されることを確認

**クローズ条件:**
- [ ] React Testing Library がセットアップされている
- [ ] `App.test.tsx` が作成され、テストが成功する
- [ ] `npm run test` でReactコンポーネントのテストが実行できる
- [ ] CIでReactテストが自動実行される
- [ ] 既存アプリとReactアプリの両方が正常に動作する

**依存関係:** Issue #10

**推定作業時間:** 2-3時間

---

## Phase 8: Reactへの段階的移行

### Issue #12: SVGCard コンポーネントの実装

**タイトル:** SVGCard コンポーネントの実装

**背景:**
Reactへの移行を段階的に進めるため、まずカード表示をReactコンポーネントとして実装します。この段階ではプレゼンテーションロジックのみを担当し、ゲームロジックとは分離します。

**作業内容:**
- `src/presentation/components/Card/Card.tsx` を作成
  - Props: `pattern`, `status`, `onClick`
  - SVG表示のみを担当（ロジックなし）
- `src/presentation/components/Card/Card.test.tsx` を作成
  - レンダリングテスト
  - クリックイベントテスト
  - 状態表示のテスト
- `src/presentation/components/Card/Card.module.css` を作成（スタイル）
- Storybookまたは単独ページで表示確認

**クローズ条件:**
- [ ] `Card.tsx` が作成され、適切にレンダリングされる
- [ ] `Card.test.tsx` が作成され、すべてのテストが成功する
- [ ] CSS Modulesでスタイルが適用されている
- [ ] まだゲーム全体には統合しない（コンポーネント単体で動作確認）
- [ ] 既存アプリは引き続き正常に動作する

**依存関係:** Issue #11

**推定作業時間:** 3-4時間

---

### Issue #13: useGame カスタムフックの実装

**タイトル:** useGame カスタムフックの実装

**背景:**
Reactでゲーム状態を管理するため、カスタムフックを実装します。Domain層の`Game`エンティティを活用し、UIとロジックを分離したまま、React特有の状態管理を実現します。

**作業内容:**
- `src/presentation/hooks/useGame.ts` を作成
  - Domain層の`Game`エンティティを利用
  - 状態管理: `cards`, `isProcessing`, `isCompleted`
  - メソッド: `handleCardClick`, `resetGame`
- `src/presentation/hooks/useGame.test.ts` を作成
  - フックの振る舞いテスト（`@testing-library/react-hooks` 使用）
  - 状態遷移のテスト
- まだUIには統合しない（フック単体のテスト）

**クローズ条件:**
- [ ] `useGame.ts` が作成され、適切に実装されている
- [ ] `useGame.test.ts` が作成され、すべてのテストが成功する
- [ ] Domain層の`Game`エンティティを利用している
- [ ] まだUIには統合しない（単体テストで動作確認）
- [ ] 既存アプリは引き続き正常に動作する

**依存関係:** Issue #9, Issue #11

**推定作業時間:** 3-4時間

---

### Issue #14: GameBoard コンポーネントの実装

**タイトル:** GameBoard コンポーネントの実装

**背景:**
カードコンポーネントとゲームフックが完成したので、これらを統合してゲームボード全体を実装します。この段階で、Reactで動作する神経衰弱ゲームが完成します。

**作業内容:**
- `src/presentation/components/Game/GameBoard.tsx` を作成
  - `useGame` フックを使用
  - `Card` コンポーネントを配置
  - カードグリッドレイアウト
- `src/presentation/components/Game/GameBoard.test.tsx` を作成
  - レンダリングテスト
  - カードクリックのインタラクションテスト
  - ゲームフローのテスト
- `src/App.tsx` で `GameBoard` を表示
- 既存アプリと同等の機能が動作することを確認

**クローズ条件:**
- [ ] `GameBoard.tsx` が作成され、適切に実装されている
- [ ] `GameBoard.test.tsx` が作成され、すべてのテストが成功する
- [ ] `/` でReact版の神経衰弱ゲームが動作する
- [ ] 既存アプリと同等の機能（カードフリップ、マッチング判定）が動作する
- [ ] `/legacy.html` で既存アプリも引き続きアクセス可能
- [ ] GitHub Pagesで両方のバージョンが正常に動作する

**依存関係:** Issue #12, Issue #13

**推定作業時間:** 4-6時間

**備考:** このissueは作業量が多いため、必要に応じて2つのPRに分割可能

---

### Issue #15: ゲーム設定パネルの実装

**タイトル:** ゲーム設定パネルの実装

**背景:**
現在、ゲーム設定（レベル、カード枚数）はURLクエリパラメータでのみ変更可能です。UIで設定を変更できるようにすることで、ユーザビリティが向上します。

**作業内容:**
- `src/presentation/components/Settings/SettingsPanel.tsx` を作成
  - レベルセレクター（1-4）
  - カード枚数セレクター
  - リセットボタン
- `src/presentation/hooks/useGameSettings.ts` を作成
  - 設定状態管理
  - URLクエリパラメータとの同期
- `src/presentation/components/Settings/SettingsPanel.test.tsx` を作成
- `App.tsx` に `SettingsPanel` を統合

**クローズ条件:**
- [ ] `SettingsPanel.tsx` が作成され、適切に実装されている
- [ ] UIでレベルとカード枚数を変更できる
- [ ] 設定変更時にゲームがリセットされる
- [ ] URLクエリパラメータからの設定読み込みも維持される
- [ ] すべてのテストが成功する
- [ ] GitHub Pagesで正常に動作する

**依存関係:** Issue #14

**推定作業時間:** 3-4時間

---

## Phase 9: UI改善・アニメーション

### Issue #16: カードフリップアニメーションの実装

**タイトル:** カードフリップアニメーションの実装

**背景:**
現在のReact版はアニメーションがなく、カードの表示・非表示が即座に切り替わります。滑らかなフリップアニメーションを追加することで、ユーザーエクスペリエンスが大幅に向上します。

**作業内容:**
- `framer-motion` をインストール
- `Card.tsx` にフリップアニメーションを追加
  - カード回転アニメーション（3D transform）
  - フェードイン・フェードアウト
- マッチ成功時のアニメーション追加
  - スケールアニメーション
  - カラー変化
- パフォーマンステスト実施（30枚以上のカードでも滑らか）

**クローズ条件:**
- [ ] `framer-motion` がインストールされている
- [ ] カードフリップ時に滑らかなアニメーションが表示される
- [ ] マッチ成功時にセレブレーションアニメーションが表示される
- [ ] アニメーションがモバイルでも滑らかに動作する
- [ ] テストが引き続き成功する（アニメーションを考慮）
- [ ] GitHub Pagesで正常に動作する

**依存関係:** Issue #14

**推定作業時間:** 3-4時間

---

### Issue #17: ゲーム統計表示の実装

**タイトル:** ゲーム統計表示の実装

**背景:**
プレイヤーに進捗やパフォーマンスを可視化するため、ゲーム統計（タイマー、試行回数など）を表示する必要があります。これにより、ゲーム性が向上し、プレイヤーのモチベーションも高まります。

**作業内容:**
- `src/presentation/components/Game/GameStats.tsx` を作成
  - プレイ時間表示（タイマー）
  - 試行回数表示（カードフリップ回数）
  - マッチ数表示
- `src/presentation/hooks/useGameTimer.ts` を作成
- `src/presentation/components/Game/GameStats.test.tsx` を作成
- `GameBoard.tsx` に `GameStats` を統合

**クローズ条件:**
- [ ] `GameStats.tsx` が作成され、適切に実装されている
- [ ] リアルタイムでタイマーが更新される
- [ ] 試行回数が正確にカウントされる
- [ ] すべてのテストが成功する
- [ ] GitHub Pagesで正常に動作する

**依存関係:** Issue #14

**推定作業時間:** 3-4時間

---

### Issue #18: ゲームクリア画面の実装

**タイトル:** ゲームクリア画面の実装

**背景:**
ゲーム完了時のフィードバックがないため、達成感が得られません。クリア画面を実装し、プレイ結果を表示することで、ユーザーエクスペリエンスが向上します。

**作業内容:**
- `src/presentation/components/UI/Modal.tsx` を作成（汎用モーダル）
- `src/presentation/components/Game/GameCompleteModal.tsx` を作成
  - クリアメッセージ
  - プレイ時間表示
  - 試行回数表示
  - リトライボタン
- ゲーム完了判定ロジックを追加
- `GameBoard.tsx` に統合

**クローズ条件:**
- [ ] `Modal.tsx`, `GameCompleteModal.tsx` が作成されている
- [ ] すべてのカードがマッチしたときにクリア画面が表示される
- [ ] クリア画面にプレイ結果が表示される
- [ ] リトライボタンでゲームがリセットされる
- [ ] すべてのテストが成功する
- [ ] GitHub Pagesで正常に動作する

**依存関係:** Issue #17

**推定作業時間:** 3-4時間

---

## Phase 10: 追加機能・最適化

### Issue #19: レスポンシブデザインの実装

**タイトル:** レスポンシブデザインの実装

**背景:**
現在のアプリはデスクトップサイズでのみ最適化されており、モバイルデバイスでの表示が考慮されていません。レスポンシブデザインを実装することで、あらゆるデバイスで快適にプレイできるようになります。

**作業内容:**
- CSSメディアクエリを追加
- カードサイズの動的調整（画面サイズに応じて）
- グリッドレイアウトの最適化
- タッチイベントの最適化（モバイル）
- 各デバイスサイズでの動作確認
  - スマートフォン（縦・横）
  - タブレット
  - デスクトップ

**クローズ条件:**
- [ ] スマートフォンで快適にプレイできる
- [ ] タブレットで快適にプレイできる
- [ ] デスクトップで快適にプレイできる（既存）
- [ ] タッチイベントが正しく動作する
- [ ] レイアウトが崩れない
- [ ] すべてのテストが成功する
- [ ] GitHub Pagesで各デバイスから正常に動作する

**依存関係:** Issue #18

**推定作業時間:** 3-4時間

---

### Issue #20: ハイスコアシステムの実装

**タイトル:** ハイスコアシステムの実装

**背景:**
プレイヤーのモチベーションを高めるため、ハイスコア（最速クリア時間、最小試行回数）を記録・表示する機能が必要です。LocalStorageを使用して、ブラウザにスコアを保存します。

**作業内容:**
- `src/infrastructure/storage/LocalStorageRepository.ts` を作成
  - スコア保存機能
  - スコア読み込み機能
  - 設定ごとのスコア管理（レベル、カード枚数）
- `src/infrastructure/storage/LocalStorageRepository.test.ts` を作成
- `src/presentation/components/Game/HighScoreDisplay.tsx` を作成
- `GameCompleteModal.tsx` でハイスコア更新を表示
- `SettingsPanel.tsx` にハイスコア表示を追加

**クローズ条件:**
- [ ] `LocalStorageRepository.ts` が作成され、適切に実装されている
- [ ] ゲームクリア時にスコアが保存される
- [ ] ハイスコアが表示される
- [ ] ハイスコア更新時に特別な表示がされる
- [ ] 設定ごとに個別のハイスコアが管理される
- [ ] すべてのテストが成功する（LocalStorageのモック含む）
- [ ] GitHub Pagesで正常に動作する

**依存関係:** Issue #18

**推定作業時間:** 4-5時間

---

### Issue #21: ドキュメント整備とレガシーコードの削除

**タイトル:** ドキュメント整備とレガシーコードの削除

**背景:**
React版が完全に機能するようになったので、レガシーコード（`legacy.html`）を削除し、プロジェクトをクリーンにします。また、開発者が参加しやすいよう、充実したドキュメントを作成します。

**作業内容:**
- `public/legacy.html` を削除
- `README.md` を更新
  - プロジェクト概要（日本語・英語）
  - 遊び方
  - 開発環境セットアップ手順
  - アーキテクチャ説明（クリーンアーキテクチャ）
  - ディレクトリ構造
  - テスト実行方法
  - デプロイ方法
- `CONTRIBUTING.md` を作成
  - コントリビューションガイドライン
  - コーディング規約
  - PR作成手順
- `docs/architecture.md` を作成（アーキテクチャ詳細ドキュメント）
- `package.json` の `description`, `keywords` 等を更新

**クローズ条件:**
- [ ] `legacy.html` が削除されている
- [ ] `README.md` が充実した内容に更新されている
- [ ] `CONTRIBUTING.md` が作成されている
- [ ] `docs/architecture.md` が作成されている
- [ ] すべてのドキュメントが日本語と英語で記載されている（または日本語のみ）
- [ ] GitHub Pagesで `/` にアクセスするとReact版のみが表示される
- [ ] プロジェクトがクリーンな状態である

**依存関係:** Issue #20

**推定作業時間:** 4-5時間

---

## 📊 サマリー

**合計:** 21 issues

**推定総作業時間:** 65-81時間

**フェーズ別内訳:**
- Phase 1 (CI/CD): 1 issue, 1-2時間
- Phase 2 (開発環境): 2 issues, 3-5時間
- Phase 3 (コード品質): 1 issue, 2-3時間
- Phase 4 (テスト環境): 1 issue, 2-3時間
- Phase 5 (Infrastructure層): 2 issues, 5-7時間
- Phase 6 (Domain層): 2 issues, 7-9時間
- Phase 7 (React環境): 2 issues, 4-6時間
- Phase 8 (React移行): 4 issues, 13-18時間
- Phase 9 (UI改善): 3 issues, 9-12時間
- Phase 10 (追加機能): 3 issues, 11-13時間

**重要マイルストーン:**
1. Issue #1完了時: CI/CDが動作し、自動デプロイが可能
2. Issue #5完了時: テスト環境が整い、品質保証が可能
3. Issue #9完了時: Domain層が完成し、ビジネスロジックが独立
4. Issue #14完了時: React版ゲームが動作（最重要マイルストーン）
5. Issue #21完了時: プロジェクト完全モダン化完了

**各issueの特徴:**
- すべてのissueは独立してマージ可能
- 各issueマージ後もアプリケーションは動作し続ける
- Issue #14以降はReact版が動作する
- 段階的にレガシーコードから移行する設計
