# Z Ecosystem 用語集

`zintent` / `ztasks` / `zconfig` をまたいで使う用語の整理です。各ツール固有の状態遷移・Schema・Protocol は各リポジトリの文書が正本です。この文書は横断理解のための索引です。

## 共通・工程

| 用語 | 意味 | 主な所有者 |
|---|---|---|
| Z Ecosystem | `zintent`、Spec Kit、`ztasks`、`zconfig` を Artifact で接続する Human-controlled Artifact Pipeline。 | `zecosystem` |
| Human-controlled Artifact Pipeline | 人間が確認できる成果物を工程間で受け渡し、承認・確認境界を人間が握る作業モデル。 | `zecosystem` |
| Artifact | 後続工程へ渡せる検査可能な成果物。例: Approved Intent Snapshot、`spec.md`、`plan.md`、`tasks.md`、実行結果、設定変更提案。 | 工程ごと |
| Definition Artifact | 「何を作るか」を定義する Project-owned 文書。Spec Kit の `spec.md` / `plan.md` / `tasks.md` が代表例。 | Project / Spec Kit |
| Runtime State | 実行中の状態、event history、session、lock、cache など。Definition Artifact を再定義しない。 | 各ツール |
| Source of Truth | ある概念の正本を持つ場所。横断方針は `zecosystem`、各ツールのDomain契約は各ツールリポジトリ。 | 分担 |
| Core | 状態遷移、検証、永続化、不変条件を所有する決定論的な実行部。 | 各ツール |
| Frontend / TUI / CLI | 表示、入力、Coreへの要求送信を担当する利用者面。状態を勝手に確定しない。 | 各ツール |
| Agent Skill | Agent が工程を正しく編成するためのMarkdown手順。状態遷移や承認を代行しない。 | `zecosystem` / 各ツール |
| Capability | 短命・範囲限定の操作権限や確認材料。推測・再利用・直接編集しない。 | 各ツール |
| Challenge / Confirmation | 人間だけが責任あるUIで応答する承認・反映確認。Agent は代行しない。 | 各ツール |
| Actor | 操作主体を示す監査用識別子。ローカルOSユーザー名などであり、認証済みidentityとは限らない。 | 各ツール |
| Provenance | 内容・操作・actor・revision・source を結ぶ機械生成の出所記録。 | 各ツール |
| Stable ID | 表示順や文言変更に依存しない識別子。Item、Task、Change Item、Comment などに使う。 | 各ツール |
| Digest / Hash | 入力やArtifactが変わっていないことを検証する内容ハッシュ。 | 各ツール |
| Redaction | credential や機密値を永続ログ・Agent応答・表示から伏せること。 | 各ツール |

## 工程順の用語

```text
Human Request
  -> zintent Meaning Gate
  -> Approved Intent Snapshot
  -> Spec Kit Definition Pipeline
  -> spec.md / plan.md / tasks.md
  -> ztasks Execution Monitor
  -> Verification / Execution Result
  -> optional zconfig Configuration Review Boundary
```

| 用語 | 意味 | 主な所有者 |
|---|---|---|
| Human Request | 人間の自然言語依頼。まだ承認済み仕様ではない。 | Human |
| Meaning Gate | 「この解釈でよいか」を確認する境界。計画やTask生成はしない。 | `zintent` |
| Approved Intent Snapshot | 人間が確認したIntent内容を固定したimmutable Artifact。Spec Kitなど後続工程への入力になる。 | `zintent` |
| Spec Kit Definition Pipeline | Approved Intent などを project-local な `spec.md` / `plan.md` / `tasks.md` に落とす工程。 | Project / Spec Kit |
| Execution Monitor | `tasks.md` に対するAgent実行の進捗・介入・結果を監視する境界。 | `ztasks` |
| Verification | 実装後の検証。テスト、check、build、手動確認などの証跡。 | Project / Agent |
| Execution Result | 実行したTask、変更、検証結果、人間介入結果などの確認可能な結果。 | Project / `ztasks` |
| Configuration Review Boundary | 開発終盤の設定変更案を人間が項目単位で確認し、明示確認後に反映する境界。 | `zconfig` |

## zecosystem

| 用語 | 意味 |
|---|---|
| Global Policy | Z Ecosystem横断の原則、工程境界、Skill方針。 |
| Cross-tool Skill | 複数ツールを接続するSkill。例: Approved Intent Snapshot を Spec Kit へ渡す handoff。 |
| Boundary | どの工程・Repo・Toolが何を所有するかの境界。共有Runtimeや共通Libraryを安易に作らないために明示する。 |
| Contract Candidate | 複数ツールで本当に共有できる可能性がある契約候補。正本化は比較・承認後に行う。 |
| Artifact Flow | Z Ecosystem の工程順と、工程間で渡すArtifactの流れ。 |

## zintent

| 用語 | 意味 |
|---|---|
| Intent | 人間がレビューする意味解釈Artifact。Intent ID、lifecycle、source、Item、Comment、Approval参照を持つ。 |
| Intent Item | 人間が個別判断できる最小の解釈単位。statement、kind、review status、rationale、source reference などを持つ。 |
| Statement | Intent Item の本文。人間が受け入れ、編集し、または却下する対象。 |
| Review Status | Itemの判断状態。`unreviewed`、`accepted`、`edited`、`rejected`。 |
| Comment | Itemに紐づくレビューコメント。`open`、`resolved`、`withdrawn` の状態を持つ。open comment は承認をblockする。 |
| Revision | 一つのgoverned mutationで生まれるimmutableな完全状態。parent、actor、operation、timestamp、content hash を持つ。 |
| HEAD | 現在のIntent revisionとhashを指す参照。直接編集しない。 |
| Lifecycle | Intent全体の状態。代表例: `draft`、`in_review`、`review_complete`、`approved`。 |
| Review Complete | 未レビューItemやopen commentがなく、承認challengeへ進める状態。 |
| Approval | 人間がreview_complete revisionを確認し、approved revisionとsnapshotを作る操作・記録。 |
| Snapshot | 承認内容を固定したcontent-addressed immutable Artifact。reject Itemやreview commentは監査履歴に残るが承認内容には含まれない。 |
| Supersede | reject済みItemなどを直接復活させず、新しいItemで置き換える関係。 |
| Source Reference | 元入力内のどこからItemが来たかを示す参照。 |

## Spec Kit

| 用語 | 意味 |
|---|---|
| `spec.md` | ユーザー価値・機能要求・受入条件を記述する仕様Artifact。 |
| `plan.md` | 技術方針、構成、制約、検証方針を記述する計画Artifact。 |
| `tasks.md` | 実装Taskのcanonical定義。`ztasks` は読むが、checkboxを含めて書き換えない。 |
| Task Definition | `tasks.md` から読めるTaskの定義。Runtime Stateとは分離する。 |
| Project-owned | 生成物や仕様が対象ProjectのGit管理下にあり、Zツールの内部状態ではないこと。 |

## ztasks

| 用語 | 意味 |
|---|---|
| Task | `tasks.md` 由来の実行単位。依存関係とruntime lifecycleを持つ。 |
| Task Runtime | Task実行中の状態、Event、Adapter応答、介入結果。Task Definitionを上書きしない。 |
| Event | `task.started`、`task.completed`、`human.pause_requested` など、append-onlyで保存される出来事。 |
| Event Replay | Event history から現在状態を再構築すること。 |
| Source Adapter | Spec Kit `tasks.md` など外部定義を source-neutral TaskDefinition に翻訳する層。 |
| Dependency Extraction | MarkdownからTask依存を抽出し、digest-bound JSONとして扱う処理。元Markdownは変更しない。 |
| Task Lifecycle | 代表例: `pending`、`ready`、`running`、`paused`、`blocked`、`failed`、`completed`、`skipped`。 |
| Human Intervention | pause / resume / retry / stop / skip / comment などの人間要求。要求記録だけでは結果確定ではない。 |
| Agent Adapter | Pi/Codex等のAgent固有操作を隔離し、ztasks protocolへ接続する層。 |
| Acknowledgement | 人間要求がAgentまたはAdapterに実際に反映されたことを示す応答。 |
| Runtime Bootstrap | 既存のSpec Kit checkbox状態をEventとして取り込む操作。source Markdownは変更しない。 |
| JSON Lines Protocol | 1行1 JSONでFrontend/AdapterとCoreが通信するprotocol。 |

## zconfig

| 用語 | 意味 |
|---|---|
| Configuration Review | 設定変更案を人間が項目単位で確認し、最終差分を明示確認してから反映する工程。 |
| Proposal | 外部Agentが作る構造化された設定変更案。元ファイルdigest、変更項目、理由、検査結果などを含む。 |
| Change Item / 変更項目 | 一つの設定変更。stable ID、JSON Pointer path、操作種別、現在値、提案値、理由、検査結果を持つ。 |
| JSON Pointer | RFC 6901形式の設定パス。変更対象を機械的に限定する。 |
| Operation | 設定変更操作。初期対象は `add`、`replace`、`remove`。 |
| Review Session | 判断、コメント、検査結果、最終確認候補を結び付けるレビュー単位。 |
| Natural-language Comment | 特定の変更項目に紐づく人間の修正指示。ファイル全体への曖昧指示ではない。 |
| Limited Revision / 再提案 | コメント対象の変更項目IDだけを許可範囲として外部Agentに修正させること。 |
| Decision | 変更項目への採否。未決定や未知IDがある状態では最終反映しない。 |
| Final Candidate / Final Change Set | 承認済み項目だけからCoreが再計算する最終的な変更候補。 |
| External Validator | 反映前の候補を検査する登録済みコマンド。未検証の成功扱いはしない。 |
| Confirmation Token | 元ファイル、候補、提案、判断、コメント、検証結果に結び付く短命・一回限りの反映確認。 |
| Apply | 確認済み最終候補を元ファイルへ安全に反映する操作。人間の明示確認なしの自動適用は対象外。 |
| Sensitive Value | Schema等で機密扱いされる値。既定で伏せ、ログには値を残さない。 |
| Audit Log | 判断、最終プレビュー、確認、反映結果などを値なしで残す監査記録。 |

## 似た用語の違い

| 用語 | 違い |
|---|---|
| Intent vs Spec | Intentは意味解釈の承認対象。SpecはProject内で実装対象を定義する文書。 |
| Task Definition vs Task Runtime | Definitionは`tasks.md`の定義。Runtimeは実行中の状態とEvent。 |
| Approval vs Confirmation | ApprovalはIntentの意味承認。Confirmationは設定反映など直前操作の明示確認。 |
| Comment vs Rationale | Commentは未解決ならblockerになる対話項目。Rationaleは判断理由の記録。 |
| Snapshot vs Runtime State | Snapshotは承認済み内容のimmutable Artifact。Runtime Stateは実行やレビューの進行状態。 |
| Proposal vs Final Candidate | Proposalは外部Agentの案。Final Candidateは承認済み項目だけからCoreが再計算した反映候補。 |
| Agent Skill vs Tool Core | Skillは手順を案内する。Coreは検証、状態遷移、永続化、不変条件を強制する。 |

## 正本へのリンク

- `zecosystem`: [Architecture Boundaries](architecture.md), [Artifact Flow](artifact-flow.md)
- `zintent`: `docs/intent-model.md`, `specs/001-intent-review-skeleton/contracts/`
- `ztasks`: `README.md`, `protocol/README.md`, `specs/001-task-execution-control/contracts/`
- `zconfig`: `README.md`, `docs/workflow.md`, `specs/001-review-config-changes/contracts/`
