# Brief: compact-guard

## Problem
AI coding agent users and maintainers rely on long-running sessions that may be compacted or compressed by existing tools such as Codex compact, Claude Code compact, and Gemini compress. After compaction, the resumed agent can lose important intent, constraints, unresolved risks, review context, or implementation state. This makes the session harder to audit and can cause unsafe or incoherent continuation.

## Current State
Existing agent compaction features act as the compaction engine, but they do not provide a shared, model-agnostic policy layer for what must be preserved, what may be omitted, or how to judge whether a compacted summary is safe to resume from. There is no OSS-ready rubric package focused specifically on evaluating, repairing, and resuming from compacted coding-agent session summaries.

## Desired Outcome
compact-guard provides an OSS policy and rubric layer that users can apply around existing compaction features. A user should be able to prepare for compaction, evaluate a compacted summary, repair an insufficient summary, and resume a coding session with clearer preserved context and explicit safety checks.

## Approach
Use a documentation-first MVP: Markdown policies, Markdown rubrics, reusable prompts, examples, and usage docs for Codex CLI, Claude Code, and Gemini CLI. This keeps compact-guard model-agnostic and avoids building or depending on a specific compaction engine while still delivering immediately usable guidance.

## Scope
- **In**: Model-agnostic compaction preservation policies
- **In**: Rubrics for judging whether a compacted session summary is safe to continue from
- **In**: Prompts for pre-compact preparation, post-compact evaluation, repair, and resume
- **In**: Bad vs good compacted-summary examples for AI coding sessions
- **In**: Usage documentation for Codex CLI, Claude Code, and Gemini CLI workflows
- **In**: OSS repository structure suitable for publishing and community contribution
- **Out**: Performing AI agent compaction or compression itself
- **Out**: Replacing built-in compact/compress features in Codex, Claude Code, Gemini, or other agents
- **Out**: A hosted service, browser extension, or agent runtime integration in the MVP
- **Out**: Tool-specific private APIs or non-portable assumptions

## Boundary Candidates
- Policy content: what information must be preserved, may be summarized, or may be dropped
- Evaluation rubrics: pass/fail criteria, scoring, and safety thresholds for compacted summaries
- Prompt pack: reusable prompts for preparation, evaluation, repair, and resume workflows
- Examples: representative bad and good compacted summaries with annotated differences
- Tool usage docs: practical workflows for Codex CLI, Claude Code, and Gemini CLI

## Out of Boundary
- compact-guard does not implement a compaction algorithm.
- compact-guard does not claim to guarantee correctness of resumed agent behavior.
- compact-guard does not require a specific model, vendor, CLI, or coding-agent framework.
- compact-guard does not own downstream implementation changes made after resume.

## Upstream / Downstream
- **Upstream**: Existing compaction/compression behavior in AI coding agents; user-provided session context; repository-specific instructions such as AGENTS.md or equivalent project memory.
- **Downstream**: Future automation that can run the rubrics, structured checklists, tool-specific integrations, CI checks for summary quality, and community-maintained policy variants.

## Existing Spec Touchpoints
- **Extends**: None. This is a greenfield spec.
- **Adjacent**: None currently. Future specs may split out automated evaluators or tool-specific integrations if the repository grows beyond the documentation-first MVP.

## Constraints
The MVP must stay model-agnostic, vendor-neutral, and usable as Markdown without requiring code execution. It should be compatible with Codex CLI, Claude Code, and Gemini CLI workflows while avoiding assumptions that only work for one tool. The project should remain OSS-friendly, reviewable, and easy for contributors to extend.
