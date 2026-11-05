# VoiceClone 共通ルール(全エージェント必読)

**目標:** 音声を複製し、ボイスとして喋らせる。

## 作業場所
- すべての作業は GitHub リポジトリ <https://github.com/hiroshimu0113-web/VoiceClone> 上で行う。
- 成果物(コード・設定・ドキュメント・実験記録)は必ずこのリポジトリにコミットする。ローカルやチャット内だけに残さない。
- 作業開始前に `git pull`(または最新の `main` を取得)し、既存の内容と `docs/` を確認する。

## ブランチ・PR 運用
- `main` へ直接 push しない。`<役割>/<内容>` 形式のブランチを切る(例: `data/prepare-samples`、`tts/baseline-inference`)。
- 変更は Pull Request で出す。`.github/pull_request_template.md` の「概要」と「確認事項」を埋める。
- 1 PR = 1 目的。小さく出す。レビュー・マージはユーザーが判断する。
- コミットメッセージは「何をしたか」が分かる一文にする。

## 役割別の指示書
| 役割 | ファイル |
|---|---|
| 統括・計画 | [docs/agents/coordinator.md](docs/agents/coordinator.md) |
| 音声データ準備 | [docs/agents/data.md](docs/agents/data.md) |
| モデル・合成(学習/推論) | [docs/agents/tts.md](docs/agents/tts.md) |
| 評価・品質確認 | [docs/agents/qa.md](docs/agents/qa.md) |

## 担当モデルの固定
作業種別ごとにモデルを固定する。定義は `.claude/agents/` にあり、`model:` で強制される。

| 作業 | エージェント | モデル |
|---|---|---|
| 設計・計画 | planner | Opus |
| 実装 | implementer | Sonnet |
| レビュー・助言 | reviewer | Fable |
| 調査 | researcher | Haiku |

役割別の指示書との対応: planner=coordinator、implementer=data+tts、reviewer=qa。researcher の調査結果は `docs/research/` に置く。

自分の役割の指示書を読んでから作業を始める。指示書自体の修正も PR で行う。

## ディレクトリ方針(必要になった時点で作る)
- `docs/` 設計・調査・実験記録・指示書
- `data/` 音声データのメタ情報・スクリプト(**音声ファイル本体は原則コミットしない**)
- `src/` 実装コード
- `configs/` 設定ファイル
- `tests/` テスト

## 機密情報の扱い(詳細は [docs/security.md](docs/security.md))
GitHub リポジトリは機密情報の保管場所ではない。非公開でも同じ。
- APIキー・トークン・個人情報・同意書・元の音声データを、コード・設定・Issue・PR・ログ・チャットに書かない。
- キーは環境変数(ローカルは `.env`、CI は GitHub Secrets)で渡す。リポジトリには変数名だけ(`.env.example`)置く。
- 個人情報・元音声は暗号化した外部ストレージに置き、保管場所と取り扱いルールだけ `docs/security.md` に書く。
- 機密らしきものを見つけたらコミットせず、ユーザーに報告する。
- 誤ってコミット/公開したら、履歴を消すだけで済ませず、そのキーを**直ちに無効化して再発行**し、ユーザーに報告する。

## 絶対に守ること
1. **秘密情報をコミットしない**(APIキー・トークン・`.env`)。`.gitignore` を確認する。
2. **同意のない声の複製をしない。** 複製対象は本人が同意した声、または自分の声のみ。同意の根拠(誰が・いつ・どの用途)を `docs/consent.md` に記録する。他人のなりすまし・詐欺・偽情報への利用に加担しない。
3. 大きな音声・モデルファイルは Git に直接入れない(Git LFS か外部ストレージを使い、場所を `docs/` に書く)。
4. 外部ライブラリ・モデルを追加する場合は、ライセンスと信頼性を PR に書く。
5. 不明点や判断が分かれることは、推測で進めず PR / Issue に書いてユーザーに確認する。

## 引き継ぎ
作業終了時に、やったこと・未完了・次にやること・注意点を PR 説明か `docs/log/YYYY-MM-DD-<役割>.md` に残す。次のエージェントはそれを読んで再開する。
