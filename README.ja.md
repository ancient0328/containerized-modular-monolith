# Framework-Agnostic Modular Architecture (FAMA)

複数のフレームワーク、クラウドプロバイダー、パッケージマネージャー、言語をサポートするモジュラーアプリケーション構築のための包括的なアーキテクチャです。

## 言語

- [English](./README.md)
- [日本語（このドキュメント）](./README.ja.md)

## 概要

FAMA（Framework-Agnostic Modular Architecture）は、様々なクラウド環境にコンテナ化してデプロイできるアプリケーションを構築するための構造化されたアプローチを提供します。モノリシックアーキテクチャの利点（シンプルさ、開発速度）とマイクロサービスの利点（モジュール性、スケーラビリティ）を組み合わせながら、それらの欠点を回避します。

FAMAの核心的な哲学は「フレームワーク非依存性」です。これにより、チームは単一の技術スタックに縛られることなく、特定の要件に基づいて各モジュールに最適な技術を選択できます。

### なぜFAMAなのか？

- **Framework-Agnostic（フレームワーク非依存）**: 特定のフレームワークに縛られない柔軟性
- **Modular（モジュラー）**: 機能を独立したモジュールとして分割する設計思想
- **Architecture（アーキテクチャ）**: これが単なるツールやライブラリではなく、設計パターンであることを示している

この名前には以下のメリットがあります：
- 簡潔で覚えやすい（FAMAという略称も良い）
- コンテナ化を前提としておらず、より広く適用可能
- 「アーキテクチャ」という言葉で具体的な実装ではなく設計思想であることが伝わる
- 技術的な柔軟性という核心的な価値がすぐに理解できる

## ドキュメント

詳細なドキュメントは`docs`ディレクトリで利用できます：

- [英語ドキュメント](./docs/en/index.md)
- [日本語ドキュメント](./docs/ja/index.md)

## 特徴

- **フレームワーク独立性**: 各モジュールに最適なフレームワークを自由に選択可能
- **クラウド非依存アーキテクチャ**: AWS、GCP/Firebase、Azure、オンプレミスへのデプロイに対応
- **複数パッケージマネージャー対応**: npm（デフォルト）、yarn、pnpmと互換性あり
- **モノレポ管理**: 効率的なビルドのためのTurborepo統合
- **多言語対応**: 英語（デフォルト）と日本語のドキュメントとインターフェース
- **アセット同期**: モジュール間のアセット管理のための組み込み`msyn`ツール
- **モジュラー構造**: 明確に定義されたモジュール境界による関心の分離
- **コンテナ化**: Dockerベースの開発とデプロイメント
- **Infrastructure as Code**: サポートされる各クラウドプロバイダー向けのテンプレート

## ディレクトリ構造

```
framework/
├── core/                         # 共有コア機能
│   ├── api/                      # API定義とインターフェース
│   ├── auth/                     # 認証と認可
│   ├── communication/            # モジュール間通信
│   └── utils/                    # 共有ユーティリティ
├── frontend/                     # モジュール化されたフロントエンド
│   ├── core/                     # フロントエンドコア機能
│   │   ├── web/                  # Webアプリフロントエンド
│   │   │   ├── [framework-1]/    # 例: React、Svelte、Vueなど
│   │   │   ├── [framework-2]/    # 別のフレームワーク実装
│   │   │   └── [framework-n]/    # 追加のフレームワーク実装
│   │   └── mobile/               # モバイルアプリフロントエンド
│   │       ├── [framework-1]/    # 例: React Native、Flutterなど
│   │       └── [framework-2]/    # 別のフレームワーク実装
│   └── modules/                  # 機能モジュール
│       ├── registry.json         # モジュール登録情報
│       └── [module-name]/        # 各モジュール
│           ├── metadata.json     # モジュールメタデータ
│           ├── web/              # Webモジュール実装
│           │   ├── [framework-1]/# 選択したWebフレームワーク実装
│           │   ├── [framework-2]/# 別のフレームワーク実装（オプション）
│           │   └── [framework-n]/# 追加のフレームワーク実装（オプション）
│           └── mobile/           # モバイルモジュール実装
│               ├── [framework-1]/# 選択したモバイルフレームワーク実装
│               └── [framework-2]/# 別のフレームワーク実装（オプション）
├── backend/                      # モジュール化されたバックエンド
│   ├── api-gateway/              # APIゲートウェイ
│   ├── auth-service/             # 認証サービス
│   └── modules/                  # バックエンドモジュール
├── assets/                       # 共有アセット
│   ├── images/                   # オリジナル画像
│   ├── images-optimized/         # 最適化された画像
│   ├── fonts/                    # フォント
│   └── icons/                    # アイコン
├── docs/                         # ドキュメント
│   ├── api/            　　　　　  # API仕様
│   ├── architecture/             # アーキテクチャ図と説明
│   ├── diagrams/                 # 視覚的な図表
│   ├── guides/                   # ユーザーガイド
│   ├── learning/                 # 学習リソース
│   └── templates/                # テンプレート
├── scripts/                      # 開発とデプロイメントのスクリプト
{{ ... }}

### ディレクトリ構造の説明

FAMAアーキテクチャは、慎重に設計されたディレクトリ構造によって関心事を分離しています。以下にいくつかの重要な区別を示します：

#### コアと実装の区別

- **コアディレクトリ** (`framework/core/*`): フレームワークに依存しないインターフェース、型、ユーティリティを含み、すべての実装で共有できます。
- **実装ディレクトリ** (例: `framework/frontend/core/web/[framework]`): 特定のフレームワークに対する具体的な実装を含みます。

#### 重要なディレクトリの区別

##### 認証コンポーネント

- **`core/auth/`**: フレームワークに依存しない共有認証インターフェース、型、ユーティリティを含みます。これには、トークン検証ロジック、認証状態の型定義、フロントエンドとバックエンドの両方で使用できる共通認証ヘルパーが含まれます。

- **`backend/auth-service`**: バックエンドのマイクロサービスとしての認証サービスの実際の実装を含みます。これには、ユーザー認証APIエンドポイント、OAuth/OIDC連携、ユーザーデータベース接続、JWTトークンの発行/検証ロジックが含まれます。

この関係は、`backend/auth-service`が`core/auth/`で定義されたインターフェースを実装することで、関心事の分離を維持しながらフロントエンドとバックエンド間で一貫した認証インターフェースを可能にします。

##### APIコンポーネント

- **`core/api/`**: フロントエンドとバックエンド間で共有されるAPI契約、インターフェース定義、型定義を含みます。

- **`backend/api-gateway`**: リクエストを適切なバックエンドサービスにルーティングするAPIゲートウェイの実装を含みます。

この分離により、共有仕様と実際の実装の間に明確な境界が設けられ、システムがより保守しやすく、さまざまなフレームワークに適応しやすくなります。

## アセット同期ツール（msyn）

FAMAには、異なるフレームワーク実装間でアセットを管理するための強力な同期ツール「msyn」が含まれています。

### 主な機能

- **自動アセット同期**: 有効化されたすべてのモジュール間でアセットを同期
- **画像最適化**: Webとモバイルのための画像を自動的に最適化
- **監視モード**: 変更を監視してリアルタイムで同期
- **多言語インターフェース**: 日本語と英語のインターフェース

### インストール

msynは公式npmパッケージとして利用可能です：

```bash
# npmを使用
npm install msyn --save-dev

# yarnを使用
yarn add msyn --dev

# pnpmを使用
pnpm add msyn --save-dev
```

### フレームワーク別の推奨パス

msynは一般的なフレームワークの推奨パスに対応しています（例）：

- **Svelte Kit**: `static/images/`
- **Next.js**: `public/images/`
- **React Native**: `src/assets/images/`
- **Flutter**: `assets/images/`
- **Angular**: `src/assets/images/`
- **Vue.js**: `public/images/`

これらは一般的な例であり、プロジェクトの構造に合わせてカスタマイズ可能です。

### msynの基本的な使い方

```bash
# アセットの同期
npx msyn sync

# 変更の監視
npx msyn watch

# 画像の最適化
npx msyn optimize

# ヘルプの表示
npx msyn help

# 言語の変更
npx msyn lang ja  # 日本語
npx msyn lang en  # 英語
```

### 設定ファイル

設定はプロジェクトルートの `.msyn.json` に保存されます。必要に応じて手動で編集することもできます：

{{ ... }}

## 始め方

### 前提条件

- Node.js 14以上
- DockerとDocker Compose
- Git

### インストール

```bash
# リポジトリのクローン
git clone https://github.com/ancient0328/containerized-modular-monolith.git
cd containerized-modular-monolith

# npm（デフォルト）で依存関係をインストール
npm install

# または、yarnを使用
yarn

# または、pnpmを使用
pnpm install
```

### セットアップ

セットアップウィザードを実行して環境を設定します：

```bash
npm run setup
```

このウィザードでは以下の設定を行います：
1. クラウドプロバイダーの選択
2. 含めるモジュールの選択
3. 開発環境の設定

## パッケージマネージャーサポート

### npm（デフォルト）

```bash
# 依存関係のインストール
npm install

# スクリプトの実行
npm run dev

# 依存関係の追加
npm install package-name
```

### Yarn

```bash
# 依存関係のインストール
yarn

# スクリプトの実行
yarn dev

# 依存関係の追加
yarn add package-name
```

### pnpm

```bash
# 依存関係のインストール
pnpm install

# スクリプトの実行
pnpm dev

# 依存関係の追加
pnpm add package-name
```

## クラウドプロバイダーサポート

フレームワークは複数のクラウドプロバイダーの設定を想定しています：

- **AWS**: CloudFormationテンプレートとCDK設定
- **GCP/Firebase**: Terraform設定とFirebaseセットアップ
- **Azure**: ARMテンプレートとAzure DevOpsパイプライン
- **オンプレミス**: Docker ComposeとKubernetes設定

セットアップ時に希望のプロバイダーを選択するか、後で手動で設定することができます。

## Turborepo統合

フレームワークは効率的なモノレポ管理のためにTurborepoと統合されています：

```bash
# すべてのワークスペースでスクリプトを実行
npx turbo run dev

# 特定のフレームワーク実装でスクリプトを実行
npx turbo run build --filter=frontend/core/web/[framework-1]

# 特定のモジュールでスクリプトを実行
npx turbo run build --filter=frontend/modules/[module-name]/web/[framework-2]
```

## 開発ワークフロー

1. **新しいモジュールの作成**: `templates/`ディレクトリからテンプレートを使用
2. **ローカル開発**: Docker Composeで実行
3. **アセットの同期**: msynツールを使用して共有アセットを各フレームワーク実装に同期
4. **テスト**: 個々のモジュールまたはアプリケーション全体のテストを実行
5. **デプロイ**: クラウド固有のデプロイメントスクリプトを使用

## マルチフレームワーク対応

このフレームワークは、複数のフロントエンドフレームワークでの実装をサポートしています。プロジェクトの要件に応じて、以下のようなフレームワークから選択できます：

- **Web**: React、Svelte、Vue、Angular、その他のモダンWebフレームワーク
- **モバイル**: React Native、Flutter、その他のクロスプラットフォームフレームワーク

各モジュールは、プロジェクトで採用するフレームワークごとに実装を提供できます。モジュールのメタデータ（`metadata.json`）には、対応するフレームワークとその実装パスが記述されます。

アプリケーションシェル（ダッシュボード）は、モジュールレジストリ（`registry.json`）を参照して利用可能なモジュールを検出し、適切なフレームワーク実装を動的に読み込みます。これにより、ユーザーは一貫したインターフェースを通じて、異なるフレームワークで実装されたモジュールにシームレスにアクセスできます。

### フレームワーク選択の自由

このアーキテクチャは、特定のフレームワークに縛られない柔軟性を提供します。例えば：

- WebはSvelteKit、モバイルはCapacitor
- WebはNext.js、モバイルはFlutter
- WebはNuxt.js、モバイルはSwift/Kotlin

など、プロジェクトやチームのニーズに最適な組み合わせを自由に選択できます。さらに、モジュールごとに異なるフレームワークを採用することも可能です。

重要なのは「対応可能性」であり、「すべてを同時に実装する必要がある」ということではありません。

### 技術選定の柔軟性

このアーキテクチャは特定のフレームワークに依存せず、プロジェクトの要件やチームのスキルセットに応じて最適な技術を選択できます。例えば：

- パフォーマンスが重視される部分にはSvelteを使用
- 複雑なUIコンポーネントにはReactを使用
- モバイルアプリにはFlutterを使用

など、モジュールごとに最適なフレームワークを選択することが可能です。

### 実装戦略

マルチフレームワーク環境での開発には、以下の戦略が有効です：

1. **モジュール間通信の標準化**: RESTful APIやGraphQLなど標準的な通信プロトコルを使用
2. **共通インターフェースの定義**: 各モジュールが実装すべきインターフェースを明確に定義
3. **メタデータ駆動アプローチ**: モジュールの機能や依存関係をメタデータで記述
4. **マイクロフロントエンド手法の活用**: WebComponentsやモジュールフェデレーションなどの技術を使用
5. **共通状態管理**: フレームワーク間で状態を共有するための仕組みを提供

これらの戦略により、異なるフレームワークで実装されたモジュールが連携して動作する統合環境を実現できます。

## 統合ポイント

異なるフレームワーク間の統合は、以下の方法で実現されます：

1. **共通APIインターフェース**: 標準化されたAPI契約によるコミュニケーション
2. **イベントバス**: フレームワークを超えたイベント伝播
3. **アセット同期ツール**: `msyn`のような実装間でアセットを共有するためのツール
4. **共有設定**: 一貫した動作のための共通設定

このアプローチにより、アプリケーションの各部分に最も適切な技術を使用しながら、システム全体の一貫性を維持することができます。

## ビジネス価値

この柔軟性は、各機能に最適な技術を選択できるだけでなく、以下のことも可能にします：

- 市場の変化や新技術の登場に迅速に対応
- チームの異なるフレームワークに関する専門知識を活用
- レガシーシステムからの段階的な移行
- 特殊なフレームワークを必要な場所で使用してパフォーマンスを最適化

## ライセンス

MIT

---
