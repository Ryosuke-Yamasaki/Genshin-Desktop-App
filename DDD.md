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

## ユビキタス言語定義

| 用語                                | 説明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| **聖遺物（Artifact）**              | 原神における装備品。5 つの部位に分類され、キャラに装備される |
| **部位（type）**                    | 聖遺物の部位。花・羽・砂・杯・冠の 5 種類がある              |
| **メインスオプション（Main Stat）** | 聖遺物に 1 つだけ付与される主要能力値                        |
| **サブオプション（Sub Stat）**      | 最大 4 つまで付与される追加能力値                            |
| **スコア（Score）**                 | 聖遺物の性能を定量化した独自指標サブオプションから算出       |
| **OCR 結果（OcrResult）**           | Tess4J で画像から抽出した文字情報                            |
| **永続化（Persistence）**           | データベース（SQLite）への保存操作                           |

---

## 目的

自身が保有している聖遺物を効率的に管理を支援するデスクトップアプリケーション。

---

## ユースケース

### 登録シナリオ

1. ユーザーがゲーム内の聖遺物のスクリーンショットを GUI でアップロード
2. アプリケーション層のサービスクラスが、画像を前処理
3. アプリケーション層のサービスクラスが、OCR サービス（Tess4J）を呼び出し、聖遺物情報を文字列として抽出
4. アプリケーション層のサービスクラスが、OCR 結果をフィールドごとに分類
5. アプリケーション層のサービスクラスが、各フィールドに対して補正処理
6. アプリケーション層のマッパークラスが、補正済みの OCR 結果を DTO に変換
7. アプリケーション層のファクトリクラスが、聖遺物エンティティを生成
8. 聖遺物エンティティを生成時に、ドメイン層のサービスクラスが、バリデーションチェック
9. 聖遺物エンティティを生成時に、ドメイン層のサービスクラスが、スコアを計算
10. 永続化層のリポジトリクラスが、聖遺物エンティティを SQLite DB に登録
11. アプリケーション層のユースケースクラスが、EventBus を通じて「聖遺物登録完了イベント」を発行し、GUI に通知

### 編集シナリオ

1. ユーザーが GUI 上で編集対象の聖遺物を選択
2. 永続化層のリポジトリクラスが、対象聖遺物を取得
3. プレゼンテーション層のマッパークラスが、DTO を ViewModel に変換
4. ユーザーが GUI 上で編集
5. プレゼンテーション層のマッパークラスが、ViewModel を DTO に変換
6. アプリケーション層のアセンブリクラスが、聖遺物エンティティを差分更新
7. 聖遺物エンティティを差分更新時に、ドメイン層のサービスクラスが、バリデーションチェック
8. 聖遺物エンティティを差分更新時に、ドメイン層のサービスクラスが、スコアを計算
9. 永続化層のリポジトリクラスが、聖遺物エンティティを SQLite DB に登録
10. アプリケーション層のユースケースクラスが、EventBus を通じて「聖遺物編集完了イベント」を発行し、GUI に通知

### 削除シナリオ

1. ユーザーが GUI 上で削除対象の聖遺物を選択
2. 永続化層のリポジトリクラスが、対象聖遺物を削除
3. アプリケーション層のユースケースクラスが、EventBus により「聖遺物削除完了イベント」を発行し、GUI に通知

### 表示シナリオ

1. ユーザーが GUI 上で聖遺物一覧表示画面を選択
2. 永続化層のリポジトリクラスが、聖遺物を全取得
3. プレゼンテーション層のマッパークラスが、DTO を ViewModel に変換

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
    │       │   ├── converter
    │       │   ├── model
    │       │   ├── service
    │       │   └── usecase
    │       ├── config
    │       ├── domain
    │       │   ├── factory
    │       │   ├── model
    │       │   │   ├── entity
    │       │   │   ├── shared
    │       │   │   └── vo
    │       │   ├── repository
    │       │   └── service
    │       ├── infrastructure
    │       │   ├── converter
    │       │   ├── model
    │       │   └── persistence
    │       ├── ocr
    │       │   ├── converter
    │       │   ├── model
    │       │   └── service
    │       └── presentation
    │           ├── controller
    │           ├── event
    │           ├── converter
    │           ├── model
    │           ├── presenter
    │           └── view
    └── resources
        ├── db
        └── tessdata
```
