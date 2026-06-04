# Research & Design Decisions

## Summary
- **Feature**: `compact-guard`
- **Discovery Scope**: New Feature
- **Key Findings**:
  - MVP は runtime を持たない Markdown documentation package として成立する。
  - Codex CLI、Claude Code、Gemini CLI には既存の compact / compress 系ワークフローがあるため、compact-guard はそれらの前後に置く policy / rubric / prompt layer として設計できる。
  - 将来の自動 evaluator や CLI は downstream であり、MVP に含めると非目標と衝突する。

## Research Log

### 既存 compact / compress workflow の確認
- **Context**: tool-specific usage docs が、既存の built-in compact / compress 機能の周辺に compact-guard を配置できるか確認した。
- **Sources Consulted**:
  - OpenAI Codex CLI slash commands: https://developers.openai.com/codex/cli/slash-commands
  - Anthropic Claude Code slash commands: https://docs.anthropic.com/en/docs/claude-code/slash-commands
  - Google Gemini CLI commands: https://google-gemini.github.io/gemini-cli/docs/cli/commands/
- **Findings**:
  - Codex CLI は `/compact` による会話圧縮を案内している。
  - Claude Code は `/compact [instructions]` による context compaction を案内している。
  - Gemini CLI は `/compress` による context replacement with summary を案内している。
- **Implications**:
  - compact-guard は各 tool の内部挙動を所有せず、pre-compact、post-compact evaluation、repair、resume の Markdown 手順を tool-specific docs に配置する。
  - tool docs は公式挙動に依存しすぎず、「既存 compact / compress を実行する前後に使う」抽象手順を主に記述する。

### Documentation-first OSS package
- **Context**: 要件は runtime code、AI API integration、hosted service、CLI automation を明示的に除外している。
- **Sources Consulted**: `.kiro/specs/compact-guard/requirements.md`
- **Findings**:
  - すべての必須成果物は Markdown で表現できる。
  - evaluator や repair を自動化する必要はなく、人間または既存 agent に提示する rubric / prompt として提供できる。
- **Implications**:
  - design.md の Technology Stack は Markdown repository と静的 docs に限定する。
  - テスト戦略は runtime test ではなく、requirements traceability、リンク整合、non-goal drift、prompt completeness のレビューに寄せる。

## Architecture Pattern Evaluation

| Option | Description | Strengths | Risks / Limitations | Notes |
|--------|-------------|-----------|---------------------|-------|
| Documentation package | Markdown policies, rubrics, prompts, examples, and usage docs as first-class artifacts | MVP 要件と完全に一致し、vendor-neutral で導入が容易 | 自動評価や実行時保証は提供しない | 採用 |
| CLI wrapper | compact / compress 前後に prompts や rubrics を実行する CLI を作る | 将来は一貫した UX を提供できる | MVP の non-goals である CLI automation と runtime dependency に近い | 不採用 |
| Hosted evaluator | compacted summary を hosted service に送って評価する | 導入体験を統一できる | AI API / hosted service / privacy concern が発生する | 不採用 |
| Vendor-specific packs | Codex、Claude、Gemini ごとに別 policy を持つ | 各 tool に最適化しやすい | vendor-neutral と model-agnostic を弱め、重複が増える | usage docs のみ tool-specific に限定 |

## Design Decisions

### Decision: Markdown documentation package を唯一の MVP artifact にする
- **Context**: compact-guard は compaction engine ではなく policy / rubric layer である。
- **Alternatives Considered**:
  1. Runtime CLI を作る。
  2. Hosted evaluator を作る。
  3. Markdown docs のみを提供する。
- **Selected Approach**: Markdown docs のみを提供する。
- **Rationale**: 要件の非目標を守りながら、policy、rubric、prompt、examples、usage docs をすべて満たせる。
- **Trade-offs**: 自動実行や機械的 enforcement は提供しないが、reviewable で vendor-neutral な OSS MVP になる。
- **Follow-up**: 将来 automated evaluator を検討する場合は別 spec として扱う。

### Decision: 共通 core docs と tool-specific usage docs を分離する
- **Context**: Codex、Claude、Gemini の workflow は名称や操作が異なるが、preserve / evaluate / repair / resume の考え方は共通である。
- **Alternatives Considered**:
  1. すべてを tool 別 docs に複製する。
  2. 共通 docs のみで tool docs を作らない。
  3. 共通 core docs と薄い tool-specific docs に分ける。
- **Selected Approach**: 共通 policy / rubric / prompt / examples を authoritative source とし、tool-specific usage docs は適用位置を説明する。
- **Rationale**: 重複を抑えつつ、各 CLI 利用者が自分の workflow へ適用できる。
- **Trade-offs**: tool docs は公式 command の変化に追随する必要がある。
- **Follow-up**: tool-specific docs の公式リンクを定期的に確認する。

### Decision: 例は tool-neutral を基本にする
- **Context**: examples は品質基準の理解が目的であり、特定 tool の挙動を保証するものではない。
- **Alternatives Considered**:
  1. Codex / Claude / Gemini それぞれに専用 examples を作る。
  2. tool-neutral examples のみを作る。
- **Selected Approach**: core examples は tool-neutral にし、tool-specific な例は usage docs 内に閉じる。
- **Rationale**: model-agnostic と vendor-neutral を保ち、criteria の理解に集中できる。
- **Trade-offs**: 特定 tool の細かな UX は usage docs 側で補う必要がある。
- **Follow-up**: 初期 examples が rubric criteria を十分にカバーしているかレビューする。

## Risks & Mitigations
- Tool-specific docs が built-in compact / compress を置き換えるように読まれるリスク — non-goals と usage docs 冒頭で明確に否定する。
- Rubric が抽象的すぎて実用判断に使えないリスク — pass / repair-needed / unsafe-to-resume の判定条件と annotated examples をセットで提供する。
- Prompt が agent-specific になり vendor-neutral を損なうリスク — prompt 本文は model-agnostic にし、tool-specific 操作は usage docs に分離する。
- Markdown package だけでは品質保証が弱いリスク — traceability review、リンク確認、non-goal drift review を testing strategy に含める。

## References
- [OpenAI Codex CLI slash commands](https://developers.openai.com/codex/cli/slash-commands) — Codex `/compact` workflow の公式参照。
- [Anthropic Claude Code slash commands](https://docs.anthropic.com/en/docs/claude-code/slash-commands) — Claude Code `/compact [instructions]` workflow の公式参照。
- [Google Gemini CLI commands](https://google-gemini.github.io/gemini-cli/docs/cli/commands/) — Gemini CLI `/compress` workflow の公式参照。
