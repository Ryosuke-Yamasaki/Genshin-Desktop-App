# GenshinDesktopApp

## 概要

### 背景

- 聖遺物が多くなり管理することができなくなった
- 自分の環境での最適解を知りたい

### 問題

- マイセット登録機能がない
- 外部でのシミュレーションツールは CLI が多い
- 自分の環境下の最適解を導き出せない

### 目的

- 所持聖遺物の管理
- 特定条件下での最適解を導くことができるシミュレーターの作成

## 想定ユーザー

- 自分だけ
- GUI で操作する

## システムの概要

- 聖遺物管理システム
- 聖遺物データ抽出システム
- キャラクターマイセット管理システム
- パーティ編成管理システム
- ローテーション管理システム
- 最適聖遺物選定システム
- シミュレーション結果管理システム

## 機能要件

### ユーザーができることの一覧

- ゲーム内画像からの聖遺物データの読み込み
- JSON ファイルからの聖遺物データの読み込み
- 聖遺物管理
- キャラクターマイセット管理
- パーティ編成管理
- ローテーション管理
- 最適聖遺物の自動選定
- ダメージ計算
- シミュレーション結果管理

### ダメージ計算式

- 以下を使用するダメージ計算式とする

$$
\text{DMG} = \text{BaseDMG} \times \text{DMG Bonus Multiplier} \times \text{CRIT DMG Multiplier} \times \text{DEF Multiplier} \times \text{RES Multiplier} \times \text{Amplifying Multiplier}
$$

#### BaseDMG

$$
\text{BaseDMG} = \text{Reference Parameter} \times \text{BaseDMG Multiplier} + \text{BaseDMG Bonus}
$$

#### DMG Bonus Multiplier

$$
\text{DMG Bonus Multiplier} = 1 + \text{DMG Bonus}
$$

#### CRIT DMG Multiplier

$$
\text{CRIT DMG Multiplier} = 1 + \text{CRIT DMG}
$$

#### DEF Multiplier

$$
\text{DEF Multiplier} = \frac{Level_{Character} + 100}{\left( 1 - \text{DEF Reduction} \right) \times \left( 1 - \text{DEF Ignored} \right) \times Level_{Enemy} + 100}
$$

#### RES Multiplier

$$
\text{RES Multiplier} =
\begin{cases}
1 - \text{RES} &  \text{RES} < 0 \\
1 - \text{RES} &  0 \leq \text{RES} < 0.75\\
1 - \text{RES} &  \text{RES} \geq 0.75 \\
\end{cases}
$$

#### Amplifying Multiplier

$$
\text{Amplifying Multiplier} = \text{Reaction Multiplier} \times \text{Level Multiplier}_{Character} \left( 1 + \text{EM Bonus} + \text{Reaction Bonus} \right)
$$

$$
\text{EM Bonus} =
\begin{cases}
2.78 \times \frac{EM}{EM + 1400} \times 100 & \text{Melt } or \text{ Vaporize} \\
\frac{5 \times EM}{EM + 1200} \times 100 & \text{Aggravate } or \text{ Spread} \\
\end{cases}
$$

$$
\text{Reaction Multiplier} =
\begin{cases}
1.15 & \text{Aggravate} \\
1.25 & \text{Spread} \\
1.5 & \text{Melt}_{Cryo} or \text{Vaporize}_{Pyro} \\
2.0 & \text{Melt}_{Pyro} or \text{Vaporize}_{Cryo} \\
\end{cases}
$$

---

| 部位 | メインオプション                                                                        |
| ---- | --------------------------------------------------------------------------------------- |
| 花   | HP 実数                                                                                 |
| 羽   | 攻撃力実数                                                                              |
| 砂   | HP%</br>攻撃力%</br>防御力%</br>元素熟知</br>元素チャージ効率                           |
| 杯   | HP%</br>攻撃力%</br>防御力%</br>元素熟知</br>各元素のダメージバフ（8 種類）             |
| 冠   | HP%</br>攻撃力%</br>防御力%</br>元素熟知</br>会心率</br>会心ダメージ</br>与える治癒効果 |

## 非機能要件

### 可用性

- 使いたいときに使えればよい
- バージョン更新後の追加データはなるはやでやる

### 機能・拡張性

- ユーザは自分一人のみ
- データ量はあまり気にしていない
- 管理するデータが増えてきたら JSON 形式での保存から DB への移行もあるかも

### 運用・保守性

- 問題発生時は自分でどうにかする

### 移行性

- 新規作成なので特になし

### セキュリティ

- データファイルは見えないようにする

## 制約条件

- Java
- Swing
- Maven
- Windows11
