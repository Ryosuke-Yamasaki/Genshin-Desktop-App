# 聖遺物管理システム基本設計ドキュメント

## 概要

- 聖遺物を管理するシステムを作る

## 画面設計

## 機能設計

### 機能一覧

- [聖遺物一覧表示](functionalDefinition/artifactList.md)
- [聖遺物詳細表示](functionalDefinition/artifactDetail.md)
- [聖遺物登録](functionalDefinition/registerArtifact.md)
- [聖遺物更新](functionalDefinition/editArtifact.md)
- [聖遺物削除](functionalDefinition/removeArtifact.md)

### 入出力項目

[入出力項目はこちら](inputOutput.md)

### 処理フローチャート

[入出力項目はこちら](processFlowDiagram.md)

### データ設計

### テーブル設計

#### Artifacts

| 項目名   | 型             | 必須 | Enum | 制約                          | 説明               |
| -------- | -------------- | ---- | ---- | ----------------------------- | ------------------ |
| id       | `UUID`         | ✅   | ⬜   |                               | 一意な ID          |
| set      | `ArtifactSet`  | ✅   | ⬜   |                               | 聖遺物のセット     |
| type     | `ArtifactType` | ✅   | ✅   |                               | 聖遺物の部位       |
| mainStat | `Stat`         | ✅   | ✅   | type によって選択肢条件あり   | メインオプション   |
| subStats | `Set<Stat>`    | ✅   | ⬜   | 4 つかつ重複不可              | サブオプション     |
| rarity   | `int`          | ✅   | ⬜   |                               | レア度             |
| level    | `int`          | ✅   | ⬜   | rarity によって選択肢条件あり | 聖遺物のレベル     |
| score    | `double`       | ✅   | ⬜   |                               | 聖遺物の会心スコア |

## 未定・保留事項

- 画面レイアウト
- UI 部品
