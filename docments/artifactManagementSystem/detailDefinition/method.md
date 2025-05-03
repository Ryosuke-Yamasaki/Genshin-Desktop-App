# メソッド設計

## Artifact

| メソッド名     | 引数の型    | 戻り値の型 | 説明                                                             |
| -------------- | ----------- | ---------- | ---------------------------------------------------------------- |
| calculateScore |             | `double`   | `subStats` から会心スコアを再計算し、 `score` プロパティを更新   |
| canEquipTo     | `Character` | `boolean`  | この聖遺物を特定キャラに装備可能か判定するロジック ※必要に応じて |
| copy           |             | `Artifact` | この `Artifact` のディープコピーを作成する                       |
| toString       |             | `String`   | デバッグ・ログ用にプロパティ内容を文字列化                       |

## ArtifactSet

| メソッド名              | 引数の型       | 戻り値の型                    | 説明                                                                  |
| ----------------------- | -------------- | ----------------------------- | --------------------------------------------------------------------- |
| isAvailableArtifactType | `ArtifactType` | `boolean`                     | 指定した部位（ArtifactType）の聖遺物がこのセットに含まれているか判定  |
| getArtifactName         | `ArtifactType` | `String`                      | 指定部位に対応する聖遺物の名称を取得（例：時計 → 「魔女の炎の時刻」） |
| getEffectByCount        | `int`          | `Optional<ArtifactSetEffect>` | 指定数の装備数に応じたセット効果を取得（例：2 個装備で 2 セット効果） |
| isAvailableRarity       | `int`          | `boolean`                     | 指定されたレアリティがこのセットで使用可能かどうか判定                |
| toString                |                | `String`                      | セット名と効果の概要を文字列として返す                                |

## ArtifactSetEffect

| メソッド名   | 引数の型 | 戻り値の型 | 説明                                         |
| ------------ | -------- | ---------- | -------------------------------------------- |
| isApplicable | `int`    | `boolean`  | 指定した値でこのセット効果が適用されるか判定 |

## ArtifactType

| メソッド名      | 引数の型   | 戻り値の型         | 説明                                                   |
| --------------- | ---------- | ------------------ | ------------------------------------------------------ |
| isAvailableType | `StatProp` | `boolean`          | 指定したメインオプションがこの部位に含まれているか判定 |
| getTypeName     | `String`   | `String`           | 指定した ID に対応する聖遺物の部位名を取得             |
| toString        |            | `String`           | 部位名を文字列として返す                               |
| validate        |            | `ValidationResult` | 部位情報の整合性チェック                               |

## ArtifactInventory

| メソッド名     | 引数の型            | 戻り値の型               | 説明                                                                                |
| -------------- | ------------------- | ------------------------ | ----------------------------------------------------------------------------------- |
| addArtifact    | `Artifact`          | `boolean`                | 聖遺物を登録（同じ `id` のものがあれば追加せず `false` を返す）                     |
| editArtifact   | `UUID` , `Artifact` | `boolean`                | 指定した `id` の聖遺物を編集する（同じ `id` のものがあれば追加せず `false` を返す） |
| removeArtifact | `UUID`              | `boolean`                | 指定した `id` の聖遺物を削除する（同じ `id` のものがあれば追加せず `false` を返す） |
| validateAll    |                     | `List<ValidationResult>` | すべての聖遺物に対して `validate()` を実行し、結果一覧を返す                        |
| importFrom     | `List<Artifact>`    | `int`                    | 他のリストから聖遺物を一括でインポート（重複は除外）、件数を返す                    |
| exportToFile   | `String`            | `boolean`                | 聖遺物一覧を指定ファイルに保存する（JSON/CSV 等、後で形式指定）                     |
| loadFromFile   | `String`            | `boolean`                | 指定ファイルから聖遺物一覧を読み込み、既存データに統合する                          |
| toString       |                     | `String`                 | 現在の聖遺物一覧の内容を出力                                                        |

## ArtifactFilter

| メソッド名 | 引数の型   | 戻り値の型         | 説明                                                                             |
| ---------- | ---------- | ------------------ | -------------------------------------------------------------------------------- |
| validate   |            | `ValidationResult` | 各プロパティに対して整合性チェックを行う（存在しない部位や不正な組み合わせなど） |
| matches    | `Artifact` | `boolean`          | 指定された Artifact がこのフィルター条件に合致するかどうかを判定                 |
| isEmpty    |            | `boolean`          | フィルター条件が何も設定されていないかを判定                                     |
| clear      |            | `void`             | 全フィルター条件をリセット                                                       |

## ArtifactSorter

| メソッド名   | 引数の型              | 戻り値の型         | 説明                                                         |
| ------------ | --------------------- | ------------------ | ------------------------------------------------------------ |
| sort         |                       | `List<Artifact>`   | 設定された条件で聖遺物リストをソートし、結果を返す           |
| setSortKeys  | `List<SortCriterion>` | `void`             | ソート基準を設定する                                         |
| setAscending | `boolean`             | `void`             | 昇順または降順の設定を行う                                   |
| setArtifacts | `List<Artifact>`      | `void`             | ソート対象の聖遺物リストを設定する                           |
| clear        |                       | `void`             | ソート条件・対象をすべてリセット                             |
| validate     |                       | `ValidationResult` | ソート条件の整合性チェック（基準の重複や未指定の基準を検出） |

## ArtifactSortCriterion

| メソッド名 | 引数の型                | 戻り値の型         | 説明                                                       |
| ---------- | ----------------------- | ------------------ | ---------------------------------------------------------- |
| compare    | `Artifact` , `Artifact` | `int`              | 指定した `statProp` に基づいて 2 つの聖遺物を比較する      |
| validate   |                         | `ValidationResult` | `statProp` が未指定でないか、priority が正値であるかを確認 |
| toString   |                         | `String`           | ソート基準の説明文字列を返す（例："会心率 (優先度 1)"）    |

## ArtifactSelector

| メソッド名      | 引数の型         | 戻り値の型         | 説明                                                   |
| --------------- | ---------------- | ------------------ | ------------------------------------------------------ |
| select          | `List<Artifact>` | `List<Artifact>`   | `baseArtifacts` からフィルタとソートを行い、結果を返す |
| setFilter       | `ArtifactFilter` | `void`             | フィルタ条件を設定する                                 |
| setSorter       | `ArtifactSorter` | `void`             | ソート条件を設定する                                   |
| validateFilters |                  | `ValidationResult` | フィルタ条件が正しいかを検証し、エラーメッセージを返す |
| clearFilters    |                  | `void`             | 現在設定されているフィルタ条件をリセットする           |
| clearSorter     |                  | `void`             | 現在設定されているソート条件をリセットする             |

## ArtifactValidator

| メソッド名               | 引数の型         | 戻り値の型         | 説明                                                                     |
| ------------------------ | ---------------- | ------------------ | ------------------------------------------------------------------------ |
| validateArtifact         | `List<Artifact>` | `ValidationResult` | 聖遺物リストのバリデーションを行い、結果を `ValidationResult` として返す |
| validateArtifacts        | `Artifact`       | `ValidationResult` | 聖遺物 1 件のバリデーションを実行する                                    |
| validateMainStat         | `Artifact`       | `ValidationResult` | メインステータスの整合性チェック                                         |
| validateSubStats         | `Artifact`       | `ValidationResult` | サブステータスの妥当性検証                                               |
| validateLevel            | `Artifact`       | `ValidationResult` | レベルがレアリティに適しているかをチェック                               |
| validateRarity           | `Artifact`       | `ValidationResult` | レアリティが正しいかを検証                                               |
| validateSetCompatibility | `Artifact`       | `ValidationResult` | セットの整合性を検証                                                     |

## ArtifactRepository

| メソッド名    | 引数の型         | 戻り値の型       | 説明                                           |
| ------------- | ---------------- | ---------------- | ---------------------------------------------- |
| loadArtifacts |                  | `List<Artifact>` | JSON ファイルから聖遺物データを読み込む        |
| saveArtifacts | `List<Artifact>` | `void`           | 聖遺物データのリストを JSON ファイルに保存する |

## ArtifactDetailViewer

## ArtifactListViewer

## ArtifactEditor

## ArtifactController

| メソッド名                 | 引数の型            | 戻り値の型               | 説明                                                       |
| -------------------------- | ------------------- | ------------------------ | ---------------------------------------------------------- |
| loadArtifactsFromFile      | `String`            | `boolean`                | ファイルから聖遺物を読み込み、inventory に登録             |
| saveArtifactsToFile        | `String`            | `boolean`                | 現在の inventory をファイルへ保存（JSON または CSV）       |
| importArtifacts            | `List<Artifact>`    | `int`                    | 他の聖遺物リストからインポートし、重複を排除した件数を返す |
| getFilteredSortedArtifacts |                     | `List<Artifact>`         | 現在のフィルタ＋ソートを適用した聖遺物リストを返す         |
| validateAllArtifacts       |                     | `List<ValidationResult>` | inventory 全体に対してバリデーションを実行                 |
| setFilter                  | `ArtifactFilter`    | `void`                   | フィルタ条件を設定し、selector にも反映                    |
| setSorter                  | `ArtifactSorter`    | `void`                   | ソート条件を設定し、selector にも反映                      |
| clearFilter                |                     | `void`                   | フィルタ条件をリセットし selector も初期化                 |
| clearSorter                |                     | `void`                   | ソート条件をリセットし selector も初期化                   |
| getArtifactById            | `UUID`              | `Optional<Artifact>`     | ID に一致する聖遺物を inventory から取得                   |
| removeArtifactById         | `UUID`              | `boolean`                | 指定 ID の聖遺物を削除                                     |
| editArtifactById           | `UUID` , `Artifact` | `boolean`                | 指定 ID の聖遺物を更新                                     |
| addArtifact                | `Artifact`          | `boolean`                | 新しい聖遺物を inventory に追加                            |
| toString                   |                     | `String`                 | 現在の状態（登録数、フィルタ状態など）を文字列出力         |

## ArtifactConstants

## ValidationResult

| メソッド名 | 引数の型           | 戻り値の型 | 説明                                     |
| ---------- | ------------------ | ---------- | ---------------------------------------- |
| addError   | `String`           | `void`     | エラーメッセージを追加する               |
| hasErrors  |                    | `boolean`  | エラーが 1 件以上あるかを判定する        |
| merge      | `ValidationResult` | `void`     | 別の `ValidationResult` の内容を統合する |
| toString   |                    | `String`   | エラー内容をまとめた文字列に変換する     |

## StatProp

| メソッド名  | 引数の型       | 戻り値の型       | 説明                                                                 |
| ----------- | -------------- | ---------------- | -------------------------------------------------------------------- |
| values      |                | `List<StatProp>` | 全 `StatProp` の一覧                                                 |
| getStatProp | `String`       | `StatProp`       | コードから `StatProp` を取得                                         |
| toString    |                | `String`         | 表示名を返す                                                         |
| isPercent   |                | `boolean`        | この `prop` がパーセンテージ系のプロパティかを返す                   |
| isFlat      |                | `boolean`        | 実数値ステータスかどうかを判定                                       |
| isValidFor  | `ArtifactType` | `boolean`        | 指定した部位のメインオプションとしてこの `prop` が適切かどうかを判定 |

## BuffEffect

| メソッド名          | 引数の型 | 戻り値の型              | 説明                                                               |
| ------------------- | -------- | ----------------------- | ------------------------------------------------------------------ |
| getStackedBuffStats | `int`    | `Map<StatProp, Double>` | 指定されたスタック数に応じた合計効果量を返す（上限は `maxStacks`） |
| isStackable         |          | `boolean`               | スタック可能かどうかを返す（`maxStacks` > 1 の場合 true）          |
| isPermanent         |          | `boolean`               | 効果が永続（`duration` == 0）かどうかを返す                        |
| validate            |          | `ValidationResult`      | プロパティ（スタック数や効果値など）の整合性チェックを行う         |
| toString            |          | `String`                | 効果内容を文字列として返す（デバッグやログ用）                     |

## BuffCondition

| メソッド名  | 引数の型 | 戻り値の型 | 説明                                         |
| ----------- | -------- | ---------- | -------------------------------------------- |
| isSatisfied | `State`  | `boolean`  | 現在の状態に対してこの条件が満たされるか判定 |
| toString    |          | `String`   | 条件の説明文を返す                           |
