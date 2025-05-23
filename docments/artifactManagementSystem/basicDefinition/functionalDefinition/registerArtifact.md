# 聖遺物登録機能設計

## 機能概要

- 聖遺物を管理しやすいように登録する
- ゲーム内画像からデータを抽出して登録する
- バックアップからの復元のために JSON ファイルからのインポートもできるようにする
- プレビューでどのファイルが選択されたのかをわかりやすくする

## 入力情報

- ゲーム内の特定のスクリーンショット画像ファイル
- JSON ファイル

## 出力情報

- 選択されたファイルのプレビュー
- 登録する聖遺物の件数

## 処理フロー

### 画像ファイルから

```mermaid
stateDiagram-v2
  direction TB

  [*] --> 入力された画像ファイルを読み込む
  入力された画像ファイルを読み込む --> 入力された画像ファイルのプレビューを出力する
  入力された画像ファイルのプレビューを出力する --> 進行状況を画面に出力する
  進行状況を画面に出力する -->  画像から聖遺物データを抽出する
  画像から聖遺物データを抽出する --> バリデーションチェックをする
  バリデーションチェックをする --> データを管理ファイルに保存する
  データを管理ファイルに保存する --> 処理完了件数を更新する
  処理完了件数を更新する --> 登録結果を画面に出力する : 全てのデータ抽出が終わった場合
  処理完了件数を更新する --> 画像から聖遺物データを抽出する : 全てのデータ抽出が終わってない場合
```

### JSON ファイルから

```mermaid
stateDiagram-v2
  direction TB

  [*] --> 入力されたJSONファイルを読み込む
  入力されたJSONファイルを読み込む --> 入力されたJSONファイルのプレビューを出力する
  入力されたJSONファイルのプレビューを出力する --> バリデーションチェックをする
  バリデーションチェックをする --> データを管理ファイルに保存する
  データを管理ファイルに保存する --> 登録結果を画面に出力する
```

## エラーハンドリング

| 処理ステップ                                   | 想定されるエラー内容                                                                                                                                 |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 入力された画像ファイルを読み込む               | ファイルが存在しない / 拡張子不正</br>読み込み失敗（破損画像、形式未対応）</br>メモリ不足                                                            |
| 入力された画像ファイルのプレビューを出力する   | プレビュー表示領域が未設定 / 非表示</br>解像度や形式によりサムネイル生成に失敗                                                                       |
| 進行状況を画面に出力する                       | UI 更新処理のタイミング不備による進捗表示遅延や欠落</br>進捗カウンタ未初期化                                                                         |
| 画像から聖遺物データを抽出する                 | OCR 精度の不足（誤認識、読取漏れ）</br>レイアウト変化に対応できていない</br>キャラ名・ステータス等の解析失敗                                         |
| バリデーションチェックをする                   | 必須フィールドが欠落</br>データ型・範囲の不一致</br>重複や不正値検出                                                                                 |
| データを管理ファイルに保存する                 | ファイル書き込み権限エラー</br>フォーマット（JSON など）への変換失敗</br>同時アクセスによる競合                                                      |
| 処理完了件数を更新する                         | カウンタ変数が正しく初期化・加算されていない</br>並列処理中に不整合が発生                                                                            |
| 登録結果を画面に出力する                       | 出力用 UI のバインディング失敗</br>結果データに不正（空や null）</br>表示形式の整形ミス                                                              |
| 入力された JSON ファイルを読み込む             | ファイルが存在しない / パス誤り</br>拡張子が`.json`でない</br>文字コードの不一致（例: UTF-8 以外）</br>ファイル形式不正（例: JSON として構文エラー） |
| 入力された JSON ファイルのプレビューを出力する | プレビュー表示 UI の初期化ミス</br>JSON が巨大すぎて UI 側でトリミング不能                                                                           |
| バリデーションチェックをする                   | 必須項目の欠落</br>型の不一致（数値が文字列になっているなど）</br>不正な値（ステータス範囲外など）                                                   |
| データを管理ファイルに保存する                 | 書き込み権限不足</br>保存先ファイルがロックされている（他プロセス使用中）</br>JSON への再シリアライズ失敗                                            |
| 登録結果を画面に出力する                       | 出力 UI のバインディングミス</br>出力データの整形ミス</br>保存結果が空または null                                                                    |
