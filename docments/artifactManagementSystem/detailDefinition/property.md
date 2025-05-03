# プロパティ設計

## Artifact

| プロパティ名 | 型             | 説明                             |
| ------------ | -------------- | -------------------------------- |
| id           | `UUID`         | 内部処理用識別子                 |
| set          | `ArtifactSet`  | セット                           |
| type         | `ArtifactType` | 部位                             |
| mainStat     | `StatProp`     | メインステータス                 |
| subStats     | `Set<Stat>`    | サブステータス、4 個かつ重複禁止 |
| rarity       | `int`          | レアリティ：例 5★ など           |
| score        | `double`       | 会心スコア                       |
| imagePath    | `String`       | 画像ファイルのパス               |

## ArtifactSet

| プロパティ名      | 型                          | 説明                     |
| ----------------- | --------------------------- | ------------------------ |
| id                | `String`                    | 内部処理用識別子         |
| name              | `String`                    | セット名                 |
| includedArtifacts | `Map<ArtifactType, String>` | このセットに属する聖遺物 |
| rarities          | `Set<Integer>`              | レアリティ               |
| setEffect         | `List<ArtifactSetEffect>`   | セット装備時の効果       |

## ArtifactSetEffect

| プロパティ名 | 型                 | 説明                             |
| ------------ | ------------------ | -------------------------------- |
| count        | `int`              | セット効果が発動する装備数       |
| effects      | `List<BuffEffect>` | 実際のバフ効果（条件や持続含む） |

## ArtifactType

| プロパティ名      | 型              | 説明                             |
| ----------------- | --------------- | -------------------------------- |
| id                | `String`        | 内部処理用識別子                 |
| name              | `String`        | 部位の名前                       |
| includedMainStats | `Set<StatProp>` | この部位に属するメインオプション |

## ArtifactInventory

| プロパティ名 | 型               | 説明                       |
| ------------ | ---------------- | -------------------------- |
| artifacts    | `List<Artifact>` | 登録されている聖遺物の一覧 |

## ArtifactFilter

| プロパティ名    | 型                                 | 説明                                                     |
| --------------- | ---------------------------------- | -------------------------------------------------------- |
| targetType      | `Set<ArtifactType>`                | 対象とする部位（花・羽・時計・杯・冠など）               |
| targetMainStats | `Map<ArtifactType, Set<StatProp>>` | 部位ごとのメインステータス条件（例：杯は炎元素ダメージ） |
| targetSubStats  | `Set<StatProp>`                    | 必須とするサブステータス（例：会心率、攻撃力%など）      |
| targetSet       | `Set<ArtifactSet>`                 | 指定された聖遺物セット効果（例：炎の魔女、剣闘士など）   |
| targetRarity    | `int`                              | 聖遺物の最低レアリティ（例：5）                          |

## ArtifactSorter

| プロパティ名 | 型                    | 説明                                      |
| ------------ | --------------------- | ----------------------------------------- |
| sortKeys     | `List<SortCriterion>` | ソート基準のリスト（優先順位順）          |
| isAscending  | `boolean`             | 昇順ソート（true）か降順ソート（false）か |
| artifacts    | `List<Artifact>`      | ソート対象の聖遺物リスト                  |

## ArtifactSortCriterion

| プロパティ名 | 型                   | 説明                                   |
| ------------ | -------------------- | -------------------------------------- |
| keyType      | `SortKeyType`        | ソート対象の種類（ `Enum` 使用）       |
| statProp     | `Optional<StatProp>` | `keyType` が `STAT_PROP` のときに使う  |
| priority     | `int`                | ソート優先順位（数値が小さいほど高い） |

## ArtifactSelector

| プロパティ名  | 型               | 説明                                          |
| ------------- | ---------------- | --------------------------------------------- |
| filter        | `ArtifactFilter` | 絞り込み条件（部位、サブ OP、セット効果など） |
| sorter        | `ArtifactSorter` | 並び替え条件（スコア順、会心率順など）        |
| baseArtifacts | `List<Artifact>` | 選定対象となる元の聖遺物リスト                |

## ArtifactValidator

| プロパティ名 | 型                 | 説明                     |
| ------------ | ------------------ | ------------------------ |
| result       | `ValidationResult` | バリデーション結果を保持 |

## ArtifactRepository

| プロパティ名 | 型       | 説明                          |
| ------------ | -------- | ----------------------------- |
| filePath     | `String` | JSON ファイルのパス（保存先） |

## ArtifactDetailViewer

## ArtifactListViewer

## ArtifactEditor

## ArtifactController

| プロパティ名 | 型                  | 説明                                       |
| ------------ | ------------------- | ------------------------------------------ |
| inventory    | `ArtifactInventory` | 聖遺物の登録・管理を行うリストコンテナ     |
| selector     | `ArtifactSelector`  | フィルタ＋ソート＋選定を行うユーティリティ |
| validator    | `ArtifactValidator` | バリデーション処理ユーティリティ           |
| activeFilter | `ArtifactFilter`    | 現在適用中のフィルタ条件                   |
| activeSorter | `ArtifactSorter`    | 現在適用中のソート条件                     |

## ArtifactConstants

## ValidationResult

| プロパティ名 | 型             | 説明                                 |
| ------------ | -------------- | ------------------------------------ |
| isValid      | `boolean`      | バリデーション成功かどうか           |
| errors       | `List<String>` | バリデーションエラーのメッセージ一覧 |

## StatProp

| プロパティ名 | 型        | 説明                                         |
| ------------ | --------- | -------------------------------------------- |
| id           | `String`  | 内部処理用識別子                             |
| name         | `String`  | UI 用の表示名                                |
| isPercent    | `boolean` | 実数かパーセンテージかを判定するためにデータ |

## BuffEffect

| プロパティ名 | 型                      | 説明                                                                          |
| ------------ | ----------------------- | ----------------------------------------------------------------------------- |
| stats        | `Map<StatProp, Double>` | 効果内容（例：通常攻撃 +40%、重撃 +40%）                                      |
| maxStacks    | `int`                   | スタック最大数（単発なら 1）                                                  |
| duration     | `int`                   | 効果持続時間（0 なら永続）                                                    |
| condition    | `BuffCondition`         | 効果が発動する条件（例：シールド状態、HP50%以下など）                         |
| targetScope  | `BuffTarget`            | 誰にこのバフが適用されるか（自分、場に出てるキャラ、全員など）`Enum` にて管理 |

## BuffCondition

| プロパティ名 | 型       | 説明                                                     |
| ------------ | -------- | -------------------------------------------------------- |
| description  | `String` | 条件の人間向け説明（例：シールド状態）                   |
| type         | `String` | 条件種別（例：shield, hp_below, skill_used など）        |
| value        | `Object` | 条件に関連する追加値（例：hp_below=50、skill_used=true） |
