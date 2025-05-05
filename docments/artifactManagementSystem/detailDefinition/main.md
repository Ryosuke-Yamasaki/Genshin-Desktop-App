# 聖遺物管理システム詳細設計ドキュメント

## クラス設計

| クラス名                       | 内容                                                 |
| ------------------------------ | ---------------------------------------------------- |
| Artifact                       | 聖遺物 1 個の基本情報を保持する                      |
| ArtifactSet                    | 聖遺物セットの基本情報を保持する                     |
| ArtifactSetEffect              | 聖遺物セットの効果の基本情報を保持する               |
| ArtifactType                   | 聖遺物部位の基本情報を保持する                       |
| ArtifactInventory              | 聖遺物の登録・編集・削除を処理する                   |
| ArtifactFilter                 | 聖遺物一覧のフィルタ条件を保持する                   |
| ArtifactSorter                 | 聖遺物一覧のソート条件を保持する                     |
| ArtifactSortCriterion          | 複数並び替えや優先度を保持する                       |
| ArtifactSelector               | フィルタ・ソート条件を処理する                       |
| ArtifactSelectionController    | 条件に基づきフィルタ・ソートを実行する               |
| ArtifactScanner                | 画像ファイルから聖遺物データの抽出を処理する         |
| ArtifactValidator              | 聖遺物データの入力チェックや整合性チェックを処理する |
| ArtifactRegistrationController | 聖遺物の登録処理を実行する                           |
| ArtifactEditController         | 聖遺物の編集処理を実行する                           |
| ArtifactDeleteController       | 聖遺物の削除処理を実行する                           |
| ArtifactRepository             | 保存ファイルとのやり取りを処理する                   |
| ArtifactListViewer             | 聖遺物一覧を表示するためのクラス（UI 用）            |
| ArtifactDetailViewer           | 聖遺物個別詳細を表示するためのクラス（UI 用）        |
| ArtifactRegistrationViewer     | 聖遺物データを登録するためのクラス（UI 用）          |
| ArtifactEditViewer             | 聖遺物データを編集するためのクラス（UI 用）          |
| ArtifactDeleteViewer           | 聖遺物データを削除するためのクラス（UI 用）          |
| ArtifactConstants              | 聖遺物に関する定数を保持する                         |
| ValidationResult               | バリデーションの結果を保持する                       |
| StatProp                       | ステータスの属性を保持する                           |
| BuffEffect                     | 1 つのバフ効果を保持する                             |
| BuffCondition                  | バフの発動条件を保持する                             |

## プロパティ設計

[プロパティ設計](property.md)

## メソッド設計

- メソッド名、引数、戻り値
- 処理概要（簡易なアルゴリズム、実装フロー）
- 呼び出し元・呼び出し先（関係図があるとベスト）

## 画面部品設計（フロントエンド側）

- 各画面部品の役割・処理（ボタン、入力欄、表示部など）
- イベント処理（クリック、バリデーションなど）
- DOM 構成 or コンポーネント構成（React/Vue などを使う場合）

## ファイル構成

- ファイル名と役割一覧
- ディレクトリ構成（例：MVC やクリーンアーキテクチャの階層）

## インターフェース仕様

- 外部 API・内部 API の詳細仕様（エンドポイント・メソッド・パラメータ・戻り値）
- 非同期処理（例：非同期 API 呼び出し、ジョブ処理）の流れ

## エラーハンドリング詳細

- 発生し得るエラーごとの処理内容（例：再試行、ログ、画面表示）
- 使用する例外クラス、catch 構造

## ログ仕様

- ログ出力内容と形式（ログレベル、フォーマット、出力先）
- トレース ID やユーザー ID の扱い

## セキュリティ対策

- 入力値のサニタイズ、エスケープ
- 権限チェックの実装箇所と方式
- セッション・トークンの管理方法

## ユニットテスト設計（任意）

- テスト対象関数とその想定ケース
- モック/スタブの利用可否
- 境界値、例外系含む

## 備考・未定事項

- 保留中の内容と検討メモ
