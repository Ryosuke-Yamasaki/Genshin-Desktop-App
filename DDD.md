# アプリケーション概要 - 原神聖遺物管理システム

## 技術スタック

- Java 17
- Maven 3.9.9
- Swing
- SQLite 3.49.1.0（JDBC 経由）
- Tess4J 4.3.1（OCR 処理）

## ルール

- DDD
- 手動 DI
- クリーンアーキテクチャ
- EventBus パターン
- Mapper クラスは一方向ごとに定義
- 1 処理 1 メソッド

## ユビキタス言語定義

| 用語                                | 説明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| **聖遺物（Artifact）**              | 原神における装備品。5 つの部位に分類され、キャラに装備される |
| **部位（type）**                    | 聖遺物の部位。花・羽・砂・杯・冠の 5 種類がある              |
| **メインスオプション（Main Stat）** | 聖遺物に 1 つだけ付与される主要能力値                        |
| **サブオプション（Sub Stat）**      | 最大 4 つまで付与される追加能力値                            |
| **スコア（Score）**                 | 聖遺物の性能を定量化した独自指標                             |
| **OCR 結果（OcrResult）**           | Tess4J で画像から抽出した文字情報                            |
| **永続化（Persistence）**           | データベース（SQLite）への保存操作                           |

---

## 目的

自身が保有している聖遺物を効率的に管理を支援するデスクトップアプリケーション。

---

## ユースケース

### 登録シナリオ

1. ユーザーがゲーム内の聖遺物のスクリーンショットを GUI でアップロード
2. アプリケーション層のユースケースクラスが、画像を前処理
3. OCR サービス（Tess4J）を呼び出し、聖遺物情報を文字列として抽出
4. OCR 結果をフィールドごとに分類
5. 各フィールドに対して補正処理
6. 補正済みの各フィールドをそれぞれ値オブジェクトへ変換
7. 値オブジェクトを使用して、ドメイン層の聖遺物エンティティを生成
8. ドメインサービスでスコアを計算し、エンティティへ設定
9. バリデーションチェック
10. 永続化層のリポジトリを経由して、エンティティを SQLite DB に登録
11. EventBus を通じて「聖遺物登録完了イベント」を発行し、GUI に通知

### 編集シナリオ

1. ユーザーが GUI 上で編集したい聖遺物を選択
2. アプリケーション層がリポジトリから対象聖遺物を取得
3. ユーザーが GUI 上で編集
4. 値オブジェクトの再生成、バリデーション、再スコアリング
5. エンティティを更新し、永続化層へ再保存
6. EventBus を通じて「聖遺物編集完了イベント」を発行し、GUI に通知

### 削除シナリオ

1. ユーザーが GUI 上で削除対象の聖遺物を選択し、削除を実行
2. アプリケーション層が該当 ID のエンティティをリポジトリ経由で削除
3. EventBus により「聖遺物削除完了イベント」を発行し、GUI に通知

## ドメインモデル

### エンティティ

- クラス名: Artifact
- タイプ: record
- フィールド:
  - id : `UUID`
  - label : `ArtifactLabel`
  - set : `ArtifactSet`
  - type : `ArtifactType`
  - rarity : `Rarity`
  - mainStat : `MainStat`
  - subStats : `Set<SubStat>`
  - score : `Score`

### 値オブジェクト

- クラス名: Stat
- タイプ: interface
- メソッド:
  - prop : `StatProp`
  - param : `double`

---

- クラス名: Label
- タイプ: record
- フィールド:
  - ja : `String`
  - en : `String`

---

- クラス名: MainStat
- タイプ: record
- 実装: Stat
- フィールド:
  - prop : `StatProp`
  - param : `double`

---

- クラス名: SubStat
- タイプ: record
- 実装: Stat
- フィールド:
  - prop : `StatProp`
  - param : `double`

---

- クラス名: Rarity
- タイプ: enum

---

- クラス名: ArtifactSet
- タイプ: enum
- フィールド:
  - label : `Label`
  - rarities : `Set<Rarity>`

---

- クラス名: StatPropType
- タイプ: enum

---

- クラス名: StatProp
- タイプ: enum
- フィールド:
  - type : `StatPropType`
  - label : `Label`

---

- クラス名: ArtifactType
- タイプ: enum
- フィールド:
  - label : `Label`
  - allowedMainStatProp : `Set<StatProp>`

---

- クラス名: Score
- タイプ: record
- フィールド:
  - value : `double`

---

- クラス名: ArtifactLabel
- タイプ: enum
- フィールド:
  - set : `ArtifactSet`
  - type : `ArtifactType`
  - label : `Label`

---

## アーキテクチャ概要

- プレゼンテーション層：Swing GUI
- アプリケーション層：ユースケース実装
- ドメイン層：エンティティ／値オブジェクト／サービス
- 永続化層：SQLite + JDBC
- 外部連携：Tess4J（OCR ライブラリ）

## ディレクトリ構成

```text
src
└── main
    ├── java
    │   └── artifact
    │       ├── application
    │       │   ├── service
    │       │   └── usecase
    │       ├── config
    │       ├── domain
    │       │   ├── model
    │       │   │   ├── entity
    │       │   │   └── vo
    │       │   ├── repository
    │       │   └── service
    │       ├── infrastructure
    │       │   ├── ocr
    │       │   │   ├── mapper
    │       │   │   └── model
    │       │   └── persistence
    │       │       ├── mapper
    │       │       ├── model
    │       │       └── database
    │       └── presentation
    │           ├── controller
    │           ├── event
    │           ├── mapper
    │           ├── model
    │           ├── presenter
    │           └── view
    └── resources
        ├── db
        └── tessdata
```

## 集約

- 名称: OCR 解析集約（OcrAnalysisAggregate）
- 責務: ゲーム内スクリーンショットから聖遺物情報を抽出可能な形に加工・変換する一連の処理
- ユースケース:
  - ユーザーがゲーム内で聖遺物画面のスクリーンショットをアップロード
  - アップロードされた画像を前処理
  - OCR を用いて聖遺物エンティティに関するテキストを抽出
  - OCR 結果の誤認識を補正
  - OCR 結果を聖遺物エンティティのフィールドごとに分類
- コンポーネント:
  - ImagePreprocessor: グレースケール化、抽出項目ごとのトリミング
  - OcrProcessor: 画像からテキストを抽出
  - OcrCorrector: OCR 解析でうまく読み取れなかった単語を修正
  - ArtifactTextClassifier: 補正済みテキストを抽出項目ごとに分類

---

- 名称: 聖遺物登録集約（ArtifactRegistrationAggregate）
- 責務: ユーザーによる聖遺物の登録・バリデーション処理
- ユースケース:
  - OCR 結果の構造化
  - 聖遺物エンティティの生成
  - 聖遺物エンティティのバリデーションチェック
  - 聖遺物のスコア計算
- コンポーネント:
  - ArtifactAssembler: OCR 結果を聖遺物エンティティの各値オブジェクトに変換
  - ArtifactFactory: 各値オブジェクトから聖遺物エンティティに変換
  - ArtifactValidator: ルールに基づき構造・値の妥当性をチェック
  - ArtifactScoreCalculator: スコア計算をする

---

- 名称: 聖遺物永続化集約（ArtifactPersistenceAggregate）
- 責務: Database への読み書き
- ユースケース:
  - 聖遺物エンティティの登録・更新・削除・取得
- コンポーネント:
  - ArtifactRepository: Database への CRUD 操作
  - ArtifactEntityMapper: 聖遺物エンティティを DB 用や UI 用へ変形

---

これは今後実装予定

- 名称: 聖遺物検索集約（ArtifactSearchAggregate）
- 責務: ユーザの操作による条件付き検索・フィルタリング・ソーティング
- ユースケース:
  - 項目ごとの検索・フィルタ・ソート
- コンポーネント:
  - ArtifactSearchCriteria: 検索条件を定義

---

- 名称: 聖遺物表示集約（ArtifactDisplayAggregate）
- 責務: Swing との連携。UI 用への整形
- ユースケース:
  - 聖遺物データを一覧で表示
  - フィルタリング・ソーティングの入力受付
- コンポーネント:
  - ArtifactDisplayPanel: 聖遺物一覧のテーブル表示パネル
  - ArtifactDisplayViewModel: UI にバインドする表示用データ（DTO 形式）を保持
  - ArtifactDisplayPresenter: ドメインモデル → ViewModel への変換処理
  - ArtifactDisplayController: UI 操作（フィルタ、ソート）をアプリケーション層へ伝達
  - ArtifactDisplayFilter: 現在のフィルタ・ソート条件を保持し、Query 用に整形
  - ArtifactDisplayService: 聖遺物の一覧取得・絞り込みなどのユースケースを実行
  - ArtifactTableModel: JTable 用の `AbstractTableModel` 実装。ViewModel を元に表示制御
  - ArtifactDisplayEventListener: ユーザー操作（ソート、フィルタ入力）に応じたイベントハンドリング

---
