# skills-repository

開発作業で使う、日本語のAgent Skills集です。Git操作、コードレビュー、テスト戦略・実装、PRの動作確認、リポジトリ指示ファイルの作成、フロントエンドの実装前調査と検証・フィードバック、セキュリティ上の問題の調査を支援します。

[Agent Skills仕様](https://agentskills.io/specification)に沿って、各スキルを `SKILL.md` と必要な付属資料で構成しています。Agent Skills対応ツールの仕様に従って配置し、利用できます。特定のエージェントに依存するメタデータや表現を整理しました。

## スキル一覧

| スキル                                                                       | 用途                                                                              |
| ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| [branch-workflow](skills/branch-workflow/SKILL.md)                           | 既存の変更を保持してGitブランチを作成・切り替え                                   |
| [commit-workflow](skills/commit-workflow/SKILL.md)                           | 対象差分と検証結果を確認してコミットを作成                                        |
| [pull-request-workflow](skills/pull-request-workflow/SKILL.md)               | GitHub PRの準備・作成・更新                                                       |
| [code-review-workflow](skills/code-review-workflow/SKILL.md)                 | 差分の正確性、回帰リスク、テスト不足をレビュー                                    |
| [test-strategy](skills/test-strategy/SKILL.md)                               | 変更に必要なテスト種別とケースを選定                                              |
| [unit-test](skills/unit-test/SKILL.md)                                       | 外部環境から分離した単体テストを実装                                              |
| [component-test](skills/component-test/SKILL.md)                             | UI部品の表示とユーザー操作のテストを実装                                          |
| [integration-test](skills/integration-test/SKILL.md)                         | 複数の構成要素が連携する統合テストを実装                                          |
| [visual-regression-test](skills/visual-regression-test/SKILL.md)             | 基準画像との差分で見た目の回帰を検証                                              |
| [pr-verification-video](skills/pr-verification-video/SKILL.md)               | PRの実アプリ動作確認、動画とレポートの作成                                        |
| [repo-instructions-generator](skills/repo-instructions-generator/SKILL.md)   | 実在する設定やコードを根拠に、指定形式の指示ファイルを作成・更新                  |
| [frontend-context-engineering](skills/frontend-context-engineering/SKILL.md) | 実装前の情報収集と、根拠・未確認事項を分けた短いブリーフの作成                    |
| [frontend-harness-engineering](skills/frontend-harness-engineering/SKILL.md) | 既存の検証手段の選択・実行、失敗の切り分け・修正・再確認                          |
| [repository-security-review](skills/repository-security-review/SKILL.md)     | セキュリティ関連URLや報告とコード・設定・依存関係を照合し、該当性と成立条件を報告 |

## 構成

```text
skills-repository/
├── LICENSE
├── README.md
└── skills/
    ├── branch-workflow/
    │   └── SKILL.md
    ├── pr-verification-video/
    │   ├── SKILL.md
    │   └── references/
    │       └── report-format.md
    └── …（全14スキル）
```

`SKILL.md` のYAML frontmatterにスキル名と説明、本文に利用場面と手順を記載しています。付属資料はスキルディレクトリからの相対パスで参照します。

## 導入と利用

1. このリポジトリを取得し、利用したいスキルを選びます。
2. 利用ツールの公式ドキュメントで、スキルの配置場所、検出方法、対応形式を確認します。
3. 選んだスキルのディレクトリを、付属資料も含めて指定の場所へコピーします。`SKILL.md` だけを切り離さず、ディレクトリ名をfrontmatterの `name` と一致させます。
4. 利用ツールの手順でスキルを読み込み、対象と作業内容を指定します。自動選択や明示呼び出しの方法は利用ツールの仕様に従います。

依頼例：

```text
test-strategy を使って、今回の変更に必要なテスト種別と不足ケースを整理してください。

frontend-context-engineering を使って、対象画面の改修に必要な情報を調べ、実装前のブリーフに整理してください。

frontend-harness-engineering を使って、今回のUI変更に合った既存の検証を選び、結果と未確認事項を報告してください。

repository-security-review を使って、共有したアドバイザリがこのリポジトリに該当するか調べ、根拠と未確認事項を報告してください。

repo-instructions-generator を使って、このリポジトリの設定やコードに基づく
CLAUDE.md を作成してください。

pr-verification-video を使って、このPRを実アプリで確認し、動画とレポートを作成してください。
```

スキル間の参照はスキル名で示しています。参照先も使う場合は、対応するスキルを同じ利用環境へ配置してください。利用できない参照先はその旨を報告し、実行した扱いにしないでください。

フロントエンド向けの2スキルは独立して利用できます。`frontend-context-engineering` は読み取りによる情報収集と整理を担当し、`frontend-harness-engineering` は既存の検証手段を使った実行・フィードバックと依頼範囲内の修正を担当します。

`repository-security-review` は、共有資料とリポジトリの証拠を照合する防御的な調査・報告用です。URLの閲覧と安全なローカル静的確認の範囲で、確認済み・可能性あり・情報不足を区別します。実サービスへの攻撃や悪用、コード修正、外部への報告・連絡は行いません。

指示ファイル生成では、指定された形式・出力先を優先します。未指定なら既存の指示ファイルを使い、候補が複数なら対象を確認します。指定も既存ファイルもなければ、リポジトリルートの `AGENTS.md` を使用します。`repo-instructions-generator` を指定してください。

## 実行環境の前提

| 対象                      | 必要な機能・ツール                                                                                                    |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| リポジトリの調査・Git操作 | ファイルの読み取り、対象操作に応じた書き込み、Git、コマンド実行機能                                                   |
| GitHubのPR操作            | 対象リポジトリへのアクセスと権限、GitHub CLI・認証済みAPI・ブラウザ操作などの実行手段                                 |
| テストの実装・実行        | 対象プロジェクトのSDK、既存テストランナー、必要なテスト用DB・サービス・ブラウザなど                                   |
| 動作確認動画              | 実アプリの起動・操作・録画機能、必要な認証情報、動画変換が必要ならFFmpegなど                                          |
| 画像による回帰検証        | 画像比較ランナー、基準画像、固定したフォント・描画環境                                                                |
| セキュリティ調査          | リポジトリの読み取り機能、URLがある場合は関連資料の閲覧手段。任意の静的確認には、安全性を確認できる既存のローカル手段 |

利用環境で使えるCLI、API、ブラウザ・OS操作機能を選び、対象リポジトリの既存設定を優先します。必要な機能や権限がなければ、未実行の作業と理由を報告します。コマンドやツール名が本文にあっても、その機能が利用環境に提供されているか確認してから使用してください。

コミット候補の確認、PR説明の更新確認、動画投稿前の確認など、各スキルの承認手順に従ってください。スキルを読み込むこと自体は、外部への投稿や変更の許可を意味しません。

## 検証

スキルを更新したときは、Agent Skills仕様に沿ったYAML frontmatter、ディレクトリ名とスキル名の一致、相対リンク、スキル間の参照、利用環境に依存する記述を確認してください。

共通形式への適合と、各エージェント製品での実機動作確認は区別して扱います。スキルの検出・呼び出し、ツールへのアクセス、権限の扱いは利用環境ごとに確認してください。

## ライセンス

[MIT License](LICENSE)
