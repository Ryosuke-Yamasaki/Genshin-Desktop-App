# 聖遺物管理システム詳細設計ドキュメント

## クラス設計

| クラス名                       | 内容                                                 |
| ------------------------------ | ---------------------------------------------------- |
| Artifact                       | 聖遺物の基本情報を保持する                           |
| ArtifactSet                    | 聖遺物セットの基本情報を保持する                     |
| ArtifactSetEffect              | 聖遺物セットの効果の基本情報を保持する               |
| ArtifactType                   | 聖遺物部位の基本情報を保持する                       |
| ArtifactList                   | 聖遺物リストのデータを保持する                       |
| ArtifactManager                | 聖遺物の登録・編集・削除を処理する                   |
| ArtifactFilter                 | 聖遺物一覧のフィルタ条件を保持する                   |
| ArtifactSorter                 | 聖遺物一覧のソート条件を保持する                     |
| ArtifactSortKey                | ソート条件の詳細情報を保持する                       |
| ArtifactSelector               | フィルタ・ソート条件を処理する                       |
| ArtifactSelectionController    | 条件に基づきフィルタ・ソートを実行する               |
| ArtifactScanner                | 画像ファイルから聖遺物データの抽出を処理する         |
| ArtifactValidator              | 聖遺物データの入力チェックや整合性チェックを処理する |
| ArtifactRegistrationController | 聖遺物の登録処理を実行する                           |
| ArtifactEditController         | 聖遺物の編集処理を実行する                           |
| ArtifactDeleteController       | 聖遺物の削除処理を実行する                           |
| ArtifactRepository             | 保存ファイルとのやり取りを処理する                   |
| ArtifactListViewer             | 聖遺物一覧を表示するための画面クラス                 |
| ArtifactDetailViewer           | 聖遺物個別詳細を表示するための画面クラス             |
| ArtifactRegistrationViewer     | 聖遺物データを登録するための画面クラス               |
| ArtifactEditViewer             | 聖遺物データを編集するための画面クラス               |
| ArtifactDeleteViewer           | 聖遺物データを削除するための画面クラス               |
| ErrorListViewer                | エラー内容を表示するための画面クラス                 |
| ArtifactConstants              | 聖遺物に関する定数を保持する                         |
| ValidationResult               | バリデーションの結果を保持する                       |
| StatProp                       | ステータスの属性を保持する                           |
| BuffEffect                     | 1 つのバフ効果を保持する                             |
| BuffCondition                  | バフの発動条件を保持する                             |

## 全体ルール

- データを保持するクラス・処理するクラス・実行するクラス・表示するクラスに分ける
