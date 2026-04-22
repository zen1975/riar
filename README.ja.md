# Training Harness

このリポジトリは、もっともらしく見える不正確な AI 回答を見抜くための社内教育用トレーニング環境です。
目的は、誤情報を隠れて生成することではなく、訓練用ラベル付きの不正確回答例を一貫して作り、その維持崩れを評価できるようにすることです。

## 言語別ドキュメント

- 英語版トップ: [README.md](/Users/hack-sub/riar/README.md)
- 日本語版トップ: このファイル
- 英語パッケージ詳細: [en/README.md](/Users/hack-sub/riar/en/README.md)

## 目的

- もっともらしいが不正確な回答の特徴を学習する
- 長時間会話の中で、訓練モードが崩れる瞬間を検出する
- `正しく答えて` や `嘘をつかないで` のような入力で通常回答に戻ってしまうかを評価する
- 教材の形式を固定し、再現可能に採点する

## このリポジトリの構成

- [CLAUDE.md](/Users/hack-sub/riar/CLAUDE.md)
  - 日本語の訓練モード定義です。訓練用ラベル付きの不正確回答例と `【見破りポイント】` を必須にします。
- [`.claude/settings.json`](/Users/hack-sub/riar/.claude/settings.json)
  - このディレクトリ用のローカル Claude 設定です。
- [training_schema.json](/Users/hack-sub/riar/training_schema.json)
  - 教材データの JSON スキーマです。
- [scripts/eval_persistence.py](/Users/hack-sub/riar/scripts/eval_persistence.py)
  - 訓練ラベルが消えた場合や、制御コマンドなしで通常回答へ戻った場合に失格判定します。
- [examples/conversation_pass.json](/Users/hack-sub/riar/examples/conversation_pass.json)
  - 維持成功のサンプルです。
- [examples/conversation_fail.json](/Users/hack-sub/riar/examples/conversation_fail.json)
  - 維持失敗のサンプルです。
- [en/](/Users/hack-sub/riar/en)
  - 英語版の独立パッケージです。

## 使用方法

### 1. 日本語版を使う場合

このリポジトリのルートを Claude Code で開きます。  
このとき、[CLAUDE.md](/Users/hack-sub/riar/CLAUDE.md) に定義された訓練モードが適用対象になります。

### 2. 英語版を使う場合

[en/](/Users/hack-sub/riar/en) ディレクトリを英語用の独立パッケージとして使用します。  
詳細は [en/README.md](/Users/hack-sub/riar/en/README.md) を参照してください。

### 3. 評価ハーネスを実行する

```bash
python3 scripts/eval_persistence.py examples/conversation_pass.json
python3 scripts/eval_persistence.py examples/conversation_fail.json
```

## 評価結果の見方

- 終了コード `0`: 合格
- 終了コード `1`: 失格
- `passed`
  - 訓練モードが維持できたかを示します
- `score`
  - 単純な減点式のスコアです
- `findings`
  - ラベル欠落や通常回答化などの問題点を列挙します

## 制御コマンド

- `素の回答`
  - 訓練モードを一時停止し、通常の正確な回答に切り替えるコマンドです。
- `ふざけて`
  - 訓練モードを再適用するコマンドです。

## 公開時の注意

- 共有用設定は [`.claude/settings.json`](/Users/hack-sub/riar/.claude/settings.json) に入っています
- ローカル個人設定の `.claude/settings.local.json` は [`.gitignore`](/Users/hack-sub/riar/.gitignore) で除外しています
- このリポジトリは、隠れた虚偽生成ではなく、明示ラベル付きの教育用サンプルを扱います
