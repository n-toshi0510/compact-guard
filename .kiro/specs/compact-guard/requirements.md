# Requirements Document

## Introduction
compact-guard は、Codex compact、Claude Code compact、Gemini compress など既存の AI コーディングエージェント向け compaction / compression 機能を使う開発者、レビュアー、OSS メンテナーのための、モデル非依存なポリシーおよびルーブリック層である。

既存の compact / compress 機能は compaction engine として動作するが、長時間の AI コーディングセッションを圧縮した後に、意図、制約、未解決リスク、レビュー文脈、実装状態などの重要情報が失われていないかを判断する共通の基準が不足している。compact-guard は compaction 自体を実行せず、Markdown ベースのポリシー、評価ルーブリック、プロンプト、例、CLI 利用ドキュメントを提供することで、compact 前の準備、compact 後の評価、修復、resume を一貫して支援する。

## Boundary Context
- **In scope**: Markdown ベースの compaction policy、evaluation rubric、pre-compact / post-compact evaluation / repair / resume 用プロンプト、bad vs good compacted-summary examples、Codex CLI・Claude Code・Gemini CLI 向け利用ドキュメント、OSS として利用・レビュー・貢献しやすいドキュメント構成。
- **Out of scope**: compact-guard が compaction 自体を実行すること、既存の built-in compact / compress 機能を置き換えること、AI API を必須にすること、hosted service を提供すること、特定 vendor または特定 model に依存すること。
- **Adjacent expectations**: compact-guard は既存 AI コーディングエージェントの compact / compress 結果を入力文脈として扱い、各ツールの built-in 機能、モデル挙動、resume 後の実装結果そのものは所有しない。

## Requirements

### Requirement 1: Compaction Policy
**Objective:** As a AI コーディングエージェント利用者, I want compact 前後で保持すべき情報と省略可能な情報を判断できる Markdown policy, so that 長時間セッションの resume に必要な文脈を失いにくくできる

#### Acceptance Criteria
1. The compact-guard documentation shall define Markdown-based policies for information that must be preserved, may be summarized, and may be dropped during compaction.
2. The compact-guard documentation shall identify required preservation categories including user intent, current task status, constraints, decisions, unresolved questions, risks, modified files, commands run, verification results, and pending next steps.
3. When a user reviews the policy, the compact-guard documentation shall distinguish compaction-critical context from low-value conversational detail.
4. If a policy item affects safe resume behavior, the compact-guard documentation shall state why the item must remain visible in the compacted summary.
5. The compact-guard documentation shall keep policy language model-agnostic and vendor-neutral.

### Requirement 2: Evaluation Rubrics
**Objective:** As a レビュアーまたは resume する利用者, I want compacted summary を評価する Markdown rubric, so that 継続してよい summary か、修復が必要な summary かを判断できる

#### Acceptance Criteria
1. The compact-guard documentation shall provide Markdown-based evaluation rubrics for assessing compacted session summaries.
2. The compact-guard documentation shall define observable pass, repair-needed, and unsafe-to-resume outcomes for compacted summaries.
3. When a user applies the rubric, the compact-guard documentation shall evaluate coverage of intent, scope, state, constraints, evidence, risks, and next actions.
4. If a compacted summary omits required context, the compact-guard documentation shall direct the user to classify the summary as repair-needed or unsafe-to-resume according to the stated criteria.
5. The compact-guard documentation shall avoid requiring any AI API, hosted service, or vendor-specific evaluator to apply the rubric.

### Requirement 3: Prompt Pack
**Objective:** As a AI コーディングエージェント利用者, I want reusable prompts for compaction workflows, so that compact 前の準備、compact 後の評価、修復、resume を一貫した手順で実行できる

#### Acceptance Criteria
1. The compact-guard documentation shall provide a pre-compact prompt that asks the active agent to prepare preservation-critical session context before using an existing compact or compress feature.
2. The compact-guard documentation shall provide a post-compact evaluation prompt that asks an agent or user to evaluate a compacted summary against the compact-guard rubric.
3. The compact-guard documentation shall provide a repair prompt that asks an agent or user to produce a safer summary when the compacted summary fails required criteria.
4. The compact-guard documentation shall provide a resume prompt that helps a resumed agent continue from a compacted summary while respecting preserved constraints, unresolved risks, and next steps.
5. When prompts refer to compaction, the compact-guard documentation shall frame existing compact / compress features as the compaction engine rather than compact-guard itself.

### Requirement 4: Bad vs Good Examples
**Objective:** As a 新規利用者または contributor, I want bad and good compacted-summary examples, so that compact-guard の品質基準を具体的に理解できる

#### Acceptance Criteria
1. The compact-guard documentation shall include examples of insufficient compacted summaries and acceptable compacted summaries for AI coding sessions.
2. When an example is presented, the compact-guard documentation shall identify which policy or rubric criteria the example satisfies or violates.
3. The compact-guard documentation shall include at least one example that shows missing intent, missing implementation state, missing verification evidence, or missing unresolved risk as a resume safety problem.
4. The compact-guard documentation shall include at least one repaired example that demonstrates how a bad compacted summary can be improved for safer resume.
5. The compact-guard documentation shall keep examples tool-neutral unless the example is explicitly part of a tool-specific usage guide.

### Requirement 5: CLI Usage Documentation
**Objective:** As a Codex CLI、Claude Code、Gemini CLI の利用者, I want tool-specific usage docs, so that compact-guard を既存の compact / compress workflow に適用できる

#### Acceptance Criteria
1. The compact-guard documentation shall provide usage guidance for Codex CLI workflows.
2. The compact-guard documentation shall provide usage guidance for Claude Code workflows.
3. The compact-guard documentation shall provide usage guidance for Gemini CLI workflows.
4. When tool-specific guidance is provided, the compact-guard documentation shall show where to apply pre-compact, post-compact evaluation, repair, and resume prompts around the existing tool workflow.
5. If a tool-specific guide discusses compact or compress behavior, the compact-guard documentation shall avoid claiming that compact-guard replaces or modifies the built-in feature.

### Requirement 6: OSS Documentation Package
**Objective:** As a OSS 利用者または contributor, I want the MVP to be reviewable and reusable as a documentation package, so that I can adopt, inspect, and improve compact-guard without running software

#### Acceptance Criteria
1. The compact-guard documentation shall be usable as Markdown content without requiring code execution.
2. The compact-guard documentation shall provide clear navigation across policies, rubrics, prompts, examples, and tool usage docs.
3. When a contributor reviews the repository, the compact-guard documentation shall make MVP scope and non-goals visible from project-level documentation.
4. The compact-guard documentation shall include contribution-facing guidance for proposing policy, rubric, prompt, example, or usage-doc improvements.
5. If a user wants to adopt compact-guard manually, the compact-guard documentation shall provide enough instructions to use the MVP without a hosted service.

### Requirement 7: Explicit Non-Goals and Portability
**Objective:** As a adopter evaluating compact-guard, I want explicit boundaries and portability guarantees, so that I can understand what compact-guard does not own before relying on it

#### Acceptance Criteria
1. The compact-guard documentation shall state that compact-guard does not perform compaction itself.
2. The compact-guard documentation shall state that compact-guard does not replace built-in compact or compress features.
3. The compact-guard documentation shall state that compact-guard does not require AI APIs.
4. The compact-guard documentation shall state that compact-guard does not provide a hosted service.
5. The compact-guard documentation shall state that compact-guard does not depend on one vendor or one model.
6. If a user attempts to treat compact-guard as a compaction engine, the compact-guard documentation shall redirect the user to apply compact-guard as a policy, rubric, and prompt layer around an existing compaction workflow.
