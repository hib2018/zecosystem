# zecosystem

`zecosystem` は、`zintent` / `ztasks` / `zconfig` をまたいで利用する **Z Ecosystem 固有のグローバル要素** を管理するリポジトリです。

英語版 README は [docs/README.en.md](docs/README.en.md) を参照してください。

## 目的

このリポジトリは、特定ツール単体には属さない Z Ecosystem 共通の知識・Skill・Contract・Protocol・Schema・Capability 方針の Source of Truth です。

一方で、最初から共通ライブラリやRuntimeを作る場所ではありません。まずは、どの情報をグローバル化し、どの情報を各ツールリポジトリに残すかの管理境界を明確にします。

## 前提条件

- Z Ecosystem は、人間が確認可能な Artifact を工程間で受け渡す Human-controlled Artifact Pipeline として扱う。
- `zintent` / `ztasks` / `zconfig` は、それぞれ独立したツールリポジトリとして Domain ロジックと正規Contractを所有する。
- Agent Skill は工程を編成するが、状態遷移・検証・永続化・不変条件は各ツールの公開 CLI / Protocol / Core が所有する。
- Pi / Codex / macOS / shell などの machine・harness 固有設定は dotfiles 側に残す。
- runtime state、lock、cache、session、snapshot、credential、secret はこのリポジトリに置かない。

## 管理対象

このリポジトリで管理するもの:

- Z Ecosystem 横断の Agent 原則と工程境界
- 複数の Z ツールを接続するグローバルSkill
- 複数ツールで本当に共有すべき Contract / Protocol / Schema / Capability の方針や定義
- Architecture / Artifact Flow のドキュメント

管理しないもの:

- `zintent` / `ztasks` / `zconfig` 固有の Domain ロジック
- 各ツール固有の Skill、Schema、Protocol、Contract
- Project-local な Spec Kit prompt や生成済み `spec.md` / `plan.md` / `tasks.md`
- Pi / Codex / macOS / shell / package manager などの設定
- 実行時状態、キャッシュ、ロック、セッション、ローカルストア

## 現在の構成

```text
zecosystem/
├── skills/          # Z Ecosystem横断Skills
├── instructions/    # Agent共通原則
├── contracts/       # 共通Contract候補
├── protocols/       # 共通Protocol候補
├── schemas/         # 共通Schema候補
├── capabilities/    # Capabilityモデル候補
└── docs/            # Architecture / Artifact Flow
```

初期Skill:

- `skills/zintent/`: public `zintent` CLI/TUI を安全に利用する Meaning Gate 編成方針
- `skills/speckit-handoff/`: Approved Intent Snapshot から Project-owned Spec Kit specify workflow へ渡す方針

## 関連リポジトリとの境界

| リポジトリ | 責務 |
|---|---|
| `hib2018/zintent` | Intent review / approval のDomain、CLI/TUI、Schema、Contract、tool-specific Skill |
| `hib2018/ztasks` | Task execution monitoring、Runtime Protocol、Event、source adapter |
| `hib2018/zconfig` | Configuration review、proposal/revision/apply Protocol、Schema、security rule |
| `hib2018/dotfiles` | Mac環境、Pi/Codex等のharness設定、symlink配線 |

## Spec Kit の扱い

現時点では、各repoの `.agents/skills/speckit-*` は project-local な Spec Kit workflow として残します。

将来的に Z Ecosystem に最適化した Spec Kit profile / overlay / fork が必要になった場合は、標準Spec Kitとの差分と目的を明確にしたうえで、このリポジトリ側で方針を管理します。

## 参照ドキュメント

- [Architecture Boundaries](docs/architecture.md)
- [Artifact Flow](docs/artifact-flow.md)
- [zconfig Investigation Notes](docs/zconfig-notes.md)
- [English README](docs/README.en.md)
