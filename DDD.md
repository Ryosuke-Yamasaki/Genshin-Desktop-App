# アプリケーション概要 - 原神聖遺物管理システム

## 技術スタック

- Java 17
- Maven 3.9.9
- Swing
- SQLite 3.49.1.0（JDBC 経由）
- Tess4J 4.3.1（OCR 処理）

## ユビキタス言語定義

| 用語                              | 説明                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| **聖遺物（Artifact）**            | 原神における装備品。5 つの部位に分類され、キャラに装備される |
| **部位（Slot）**                  | 聖遺物の部位。花・羽・砂・杯・冠の 5 種類がある              |
| **メインステータス（Main Stat）** | 聖遺物に 1 つだけ付与される主要能力値                        |
| **サブステータス（Sub Stat）**    | 最大 4 つまで付与される追加能力値                            |
| **スコア（Score）**               | 聖遺物の性能を定量化した独自指標                             |
| **OCR 結果（OcrResult）**         | Tess4J で画像から抽出した文字情報                            |
| **永続化（Persistence）**         | データベース（SQLite）への保存操作                           |

---

## 目的

自身が保有している聖遺物を効率的に管理し、最適なビルド選択を支援するデスクトップアプリケーション。

---

## ユースケース

| ID   | ユースケース                     | 概要                                                             |
| ---- | -------------------------------- | ---------------------------------------------------------------- |
| UC01 | 聖遺物の画像を読み込む           | スクリーンショットを読み込み、OCR で聖遺物を抽出する             |
| UC02 | 登録済み聖遺物を取得する         | DB から登録したデータを一覧で取得                                |
| UC03 | 聖遺物を検索する                 | 部位・メインステータス・サブステータス等でフィルター検索         |
| UC04 | 聖遺物を並び替える               | スコア順、日付順などでソート表示                                 |
| UC05 | 聖遺物を編集する                 | 登録済み情報の修正（サブステ変化やスコア補正など）               |
| UC06 | 聖遺物を削除する                 | 不要になった聖遺物のデータを削除する                             |
| UC07 | 聖遺物のスコアを自動計算する     | スコアを計算する                                                 |
| UC08 | 聖遺物セットのビルド評価を行う   | 特定のキャラクターに最適な聖遺物構成を評価する（拡張可能）       |
| UC09 | 聖遺物の統計情報を表示する       | サブステ出現率や強化回数の分布などを可視化する（将来的な分析用） |
| UC10 | データのエクスポート／インポート | SQLite のデータをバックアップ・復元できるようにする              |
| UC11 | UI テーマ／設定を変更する        | Swing ベースの画面テーマや表示設定の変更                         |

---

## 🧭 コンテキストマップ（Context Map）

- 名称: 聖遺物登録コンテキスト（ArtifactRegistrationContext）
- 責務: ユーザーによる聖遺物の登録・バリデーション処理
- ユースケース:
  - OCR 結果の構造化
  - 聖遺物エンティティの生成
  - 聖遺物エンティティのバリデーションチェック
- コンポーネント:
  - ArtifactFactory: OCR 結果を聖遺物エンティティに変換
  - ArtifactValidator: ルールに基づき構造・値の妥当性をチェック
- 入力: OCR 結果（新規）、UI からの手入力（編集）
- 出力: 聖遺物永続化コンテキスト

---

- 名称: 聖遺物永続化コンテキスト（ArtifactPersistenceContext）
- 責務: Database への読み書き
- ユースケース:
  - 聖遺物エンティティの登録・更新・削除・取得
- コンポーネント:
  - ArtifactRepository: Database への CRUD 操作
  - ArtifactEntityMapper: 聖遺物エンティティを DB 用や UI 用へ変形
- 入力: 聖遺物エンティティ
- 出力: 聖遺物表示コンテキスト

---

### OCR コンテキスト

- 目的: Tess4J を用いて画像からテキスト情報を抽出する
- 責務:
  - OCR 処理
  - OCR 結果の正規化
- 外部との関係: **登録・補正コンテキスト**に正規化済みの OCR 結果を渡す

### 登録・補正コンテキスト

- 目的: 正規化済みの OCR 結果を構造化し、永続化用にデータを変換する
- 責務:
  - OCR 処理
  - OCR 結果の正規化
- 外部との関係: **登録・補正コンテキスト**に正規化済みの OCR 結果を渡す

### 永続化コンテキスト

- 目的: Tess4J を用いて画像からテキスト情報を抽出する
- 責務:
  - OCR 処理
  - OCR 結果の正規化
- 外部との関係: **登録・補正コンテキスト**に正規化済みの OCR 結果を渡す

### ドメイン評価コンテキスト

- 目的: Tess4J を用いて画像からテキスト情報を抽出する
- 責務:
  - OCR 処理
  - OCR 結果の正規化
- 外部との関係: **登録・補正コンテキスト**に正規化済みの OCR 結果を渡す

### UI コンテキスト

- 目的: Tess4J を用いて画像からテキスト情報を抽出する
- 責務:
  - OCR 処理
  - OCR 結果の正規化
- 外部との関係: **登録・補正コンテキスト**に正規化済みの OCR 結果を渡す

---

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

- クラス名: Label
- タイプ: interface
- メソッド:
  - ja : `String`
  - en : `String`

---

- クラス名: Stat
- タイプ: interface
- メソッド:
  - prop : `StatProp`
  - param : `double`

---

- クラス名: ArtifactLabel
- タイプ: record
- 実装: Label
- フィールド:
  - jpLabel : `String`
  - enLabel : `String`

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

- クラス名: ArtifactSetLabel
- タイプ: record
- 実装: Label
- フィールド:
  - jpLabel : `String`
  - enLabel : `String`

---

- クラス名: Rarity
- タイプ: record
- フィールド:
  - value : `int`

---

- クラス名: ArtifactSet
- タイプ: record
- フィールド:
  - label : `ArtifactSetLabel`
  - rarities : `Set<Rarity>`

---

- クラス名: StatPropType
- タイプ: enum

---

- クラス名: StatPropLabel
- タイプ: record
- 実装: Label
- フィールド:
  - jpLabel : `String`
  - enLabel : `String`

---

- クラス名: StatProp
- タイプ: enum
- フィールド:
  - type : `StatPropType`
  - label : `StatPropLabel`

---

- クラス名: ArtifactTypeLabel
- タイプ: record
- 実装: Label
- フィールド:
  - jpLabel : `String`
  - enLabel : `String`

---

- クラス名: ArtifactType
- タイプ: enum
- フィールド:
  - label : `ArtifactTypeLabel`
  - allowedMainStatProp : `Set<StatProp>`
- メソッド:
  - isAllowedMainStatProp :
    - 引数:
      - type: `StatProp`
    - 戻り値の型: `boolean`

---

### ⚙️ サービス（Domain Services）

#### ArtifactFilterService

- 条件に一致する聖遺物一覧を抽出
- 例：`filterBy(mainStat = "CRIT Rate", setName = "剣闘士")`

#### ArtifactRecommendationService

- 最適な聖遺物セットを提案
- キャラクターに応じたステータス加味

---

### 📚 集約（Aggregate Roots）

- `ArtifactAggregate`（Artifact + Stat + Slot の整合性を保証）
- `UserConfigAggregate`

---

## 🧩 アーキテクチャ概要

- プレゼンテーション層：Swing GUI
- アプリケーション層：ユースケース実装
- ドメイン層：エンティティ／値オブジェクト／サービス
- 永続化層：SQLite + JDBC
- 外部連携：Tess4J（OCR ライブラリ）

---
