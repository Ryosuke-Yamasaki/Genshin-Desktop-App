# データベース設計

## テーブル設計

### テーブル一覧

| テーブル名                       | 内容                                         |
| -------------------------------- | -------------------------------------------- |
| Artifacts                        | 所持している聖遺物を保持する                 |
| ArtifactSets                     | 聖遺物セットを保持する                       |
| ArtifactTypes                    | 聖遺物部位を保持する                         |
| ArtifactLabels                   | 聖遺物名を保持する                           |
| ArtifactMainStatProps            | 聖遺物部位ごとのメインステータスを保持する   |
| ArtifactSubStats                 | 所持している聖遺物のサブステータスを保持する |
| StatProps                        | ステータスの属性を保持する                   |
| CharacterMySets                  | 所持しているキャラクターを保持する           |
| Characters                       | キャラクターを保持する                       |
| Visions                          | キャラクターの神の目を保持する               |
| Nations                          | 出身国を保持する                             |
| CharacterBaseStatParams          | キャラクターの基礎ステータスを保持する       |
| CharacterAscensionStatParams     | キャラクターの突破ステータスを保持する       |
| CharacterAscensionStatParamTypes | 突破ステータスの種類を保持する               |
| Actions                          | アクションを保持する                         |
| ActionTypes                      | アクションの種類を保持する                   |
| CharacterActionMultipliers       | キャラクターの攻撃アクションの倍率を保持する |
| ReferenceParams                  | 参照パラメータを保持する                     |
| Elements                         | 元素力を保持する                             |
| Weapons                          | 武器を保持する                               |
| WeaponTypes                      | 武器の種類を保持する                         |
| WeaponBaseAttackTypes            | 武器の基礎攻撃力の種類を保持する             |
| WeaponBaseStatParams             | 武器の基礎攻撃力とステータスを保持する       |
| Parties                          | パーティを保持する                           |
| Compositions                     | パーティ内のキャラクターを保持する           |
| Rotations                        | スキルローテーションを保持する               |
| RotationSequences                | スキルローテーションの順番を保持する         |
| DamageSimulations                | シミュレーション結果を保持する               |

### カラム設計

#### Artifacts

| カラム名       | 型     | Null | 主キー | 外部キー      |
| -------------- | ------ | :--: | :----: | ------------- |
| id             | String |      |   ✅   |               |
| setId          | String |      |        | ArtifactSets  |
| typeId         | String |      |        | ArtifactTypes |
| mainStatPropId | String |      |        | StatProps     |
| score          | double |      |        |               |

#### ArtifactSets

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |
| rarity   | int    |      |        |          |

#### ArtifactTypes

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### ArtifactLabels

| カラム名 | 型     | Null | 主キー | 外部キー      |
| -------- | ------ | :--: | :----: | ------------- |
| setId    | String |      |   ✅   | ArtifactSets  |
| typeId   | String |      |   ✅   | ArtifactTypes |
| jpLabel  | String |      |        |               |
| enLabel  | String |      |        |               |

#### ArtifactMainStatProps

| カラム名   | 型     | Null | 主キー | 外部キー      |
| ---------- | ------ | :--: | :----: | ------------- |
| typeId     | String |      |   ✅   | ArtifactTypes |
| statPropId | String |      |   ✅   | StatProps     |

#### ArtifactSubStats

| カラム名   | 型     | Null | 主キー | 外部キー  |
| ---------- | ------ | :--: | :----: | --------- |
| id         | String |      |   ✅   |           |
| artifactId | String |      |        | Artifacts |
| statPropId | String |      |        | StatProps |
| statParam  | double |      |        |           |

#### StatProps

| カラム名  | 型      | Null | 主キー | 外部キー |
| --------- | ------- | :--: | :----: | -------- |
| id        | String  |      |   ✅   |          |
| jpLabel   | String  |      |        |          |
| enLabel   | String  |      |        |          |
| isPercent | boolean |      |        |          |

#### CharacterMySets

| カラム名               | 型     | Null | 主キー | 外部キー     |
| ---------------------- | ------ | :--: | :----: | ------------ |
| id                     | String |      |   ✅   |              |
| characterId            | String |      |        | Characters   |
| characterLevel         | int    |      |        |              |
| characterAscensionRank | int    |      |        |              |
| constellationRank      | int    |      |        |              |
| normalAttackLevel      | int    |      |        |              |
| elementalSkillLevel    | int    |      |        |              |
| elementalBurstLevel    | int    |      |        |              |
| weaponId               | String |      |        | Weapons      |
| weaponLevel            | int    |      |        |              |
| weaponAscensionRank    | int    |      |        |              |
| refinementRank         | int    |      |        |              |
| artifactSetId          | String |      |        | ArtifactSets |

#### Characters

| カラム名                 | 型     | Null | 主キー | 外部キー                         |
| ------------------------ | ------ | :--: | :----: | -------------------------------- |
| id                       | String |      |   ✅   |                                  |
| jpLabel                  | String |      |        |                                  |
| enLabel                  | String |      |        |                                  |
| rarity                   | int    |      |        |                                  |
| visionId                 | String |      |        | Visions                          |
| nationId                 | String |  ✅  |        | Nations                          |
| weaponTypeId             | String |      |        | WeaponTypes                      |
| ascensionStatPropId      | String |      |        | StatProps                        |
| ascensionStatParamTypeId | String |      |        | CharacterAscensionStatParamTypes |

#### Visions

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### Nations

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### CharacterBaseStatParams

| カラム名      | 型     | Null | 主キー | 外部キー   |
| ------------- | ------ | :--: | :----: | ---------- |
| id            | String |      |   ✅   |            |
| characterId   | String |      |        | Characters |
| level         | int    |      |        |            |
| ascensionRank | int    |      |        |            |
| hp            | double |      |        |            |
| attack        | double |      |        |            |
| defense       | double |      |        |            |

#### CharacterAscensionStatParams

| カラム名      | 型     | Null | 主キー | 外部キー                         |
| ------------- | ------ | :--: | :----: | -------------------------------- |
| id            | String |      |   ✅   |                                  |
| typeId        | String |      |        | CharacterAscensionStatParamTypes |
| ascensionRank | int    |      |        |                                  |
| value         | double |      |        |                                  |

#### CharacterAscensionStatParamTypes

| カラム名   | 型     | Null | 主キー | 外部キー  |
| ---------- | ------ | :--: | :----: | --------- |
| id         | String |      |        |           |
| rarity     | int    |      |   ✅   |           |
| statPropId | String |      |   ✅   | StatProps |

#### Actions

| カラム名    | 型     | Null | 主キー | 外部キー    |
| ----------- | ------ | :--: | :----: | ----------- |
| id          | String |      |   ✅   |             |
| characterId | String |  ✅  |        | Characters  |
| jpLabel     | String |      |        |             |
| enLabel     | String |      |        |             |
| typeId      | String |      |        | ActionTypes |
| coolDown    | int    |      |        |             |
| duration    | int    |      |        |             |
| castTime    | int    |      |        |             |
| hitCount    | int    |      |        |             |

#### ActionTypes

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### CharacterActionMultipliers

| カラム名          | 型     | Null | 主キー | 外部キー         |
| ----------------- | ------ | :--: | :----: | ---------------- |
| id                | String |      |   ✅   |                  |
| characterActionId | String |      |        | CharacterActions |
| level             | int    |      |        |                  |
| referenceParamId  | String |      |        | ReferenceParams  |
| elementId         | String |      |        | Elements         |
| value             | double |      |        |                  |

#### ReferenceParams

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### Elements

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### Weapons

| カラム名         | 型     | Null | 主キー | 外部キー              |
| ---------------- | ------ | :--: | :----: | --------------------- |
| id               | String |      |   ✅   |                       |
| jpLabel          | String |      |        |                       |
| enLabel          | String |      |        |                       |
| rarity           | int    |      |        |                       |
| typeId           | String |      |        | WeaponTypes           |
| statPropId       | String |      |        | StatProps             |
| baseAttackTypeId | String |      |        | WeaponBaseAttackTypes |

#### WeaponTypes

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### WeaponBaseAttackTypes

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| jpLabel  | String |      |        |          |
| enLabel  | String |      |        |          |

#### WeaponBaseStatParams

| カラム名         | 型     | Null | 主キー | 外部キー              |
| ---------------- | ------ | :--: | :----: | --------------------- |
| id               | String |      |   ✅   |                       |
| statPropId       | String |      |        | StatProps             |
| baseAttackTypeId | String |      |        | WeaponBaseAttackTypes |
| level            | int    |      |        |                       |
| ascensionRank    | int    |      |        |                       |

#### Parties

| カラム名 | 型     | Null | 主キー | 外部キー |
| -------- | ------ | :--: | :----: | -------- |
| id       | String |      |   ✅   |          |
| name     | String |  ✅  |        |          |

#### Compositions

| カラム名    | 型     | Null | 主キー | 外部キー   |
| ----------- | ------ | :--: | :----: | ---------- |
| id          | String |      |   ✅   |            |
| partyId     | String |      |        | Parties    |
| characterId | String |      |        | Characters |

#### Rotations

| カラム名      | 型     | Null | 主キー | 外部キー     |
| ------------- | ------ | :--: | :----: | ------------ |
| id            | String |      |   ✅   |              |
| compositionId | int    |      |        | Compositions |

#### RotationSequences

| カラム名       | 型     | Null | 主キー | 外部キー  |
| -------------- | ------ | :--: | :----: | --------- |
| id             | String |      |   ✅   |           |
| rotationId     | String |      |        | Rotations |
| sequenceNumber | int    |      |        |           |
| actionId       | int    |      |        | Actions   |

#### DamageSimulations

| カラム名                | 型     | Null | 主キー | 外部キー          |
| ----------------------- | ------ | :--: | :----: | ----------------- |
| id                      | String |      |   ✅   |                   |
| RotationSequenceId      | String |      |        | RotationSequences |
| enemyLevel              | int    |      |        |                   |
| enemyPyroResistance     | double |      |        |                   |
| enemyHydroResistance    | double |      |        |                   |
| enemyCryoResistance     | double |      |        |                   |
| enemyElectroResistance  | double |      |        |                   |
| enemyAnemoResistance    | double |      |        |                   |
| enemyGeoResistance      | double |      |        |                   |
| enemyDendroResistance   | double |      |        |                   |
| enemyPhysicalResistance | double |      |        |                   |
