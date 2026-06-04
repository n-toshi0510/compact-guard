# Implementation Plan

- [x] 1. Documentation package foundation
- [x] 1.1 Create the OSS entry point and navigation skeleton
  - Create the top-level project entry point that explains compact-guard as a policy and rubric layer around existing compact / compress features.
  - Create the documentation index so users can find policies, rubrics, prompts, examples, usage docs, and non-goals from one place.
  - The completed repository has visible Markdown navigation from the project entry point to every MVP artifact category.
  - _Requirements: 6.1, 6.2, 6.3, 6.5, 7.1, 7.2, 7.3, 7.4, 7.5_
  - _Boundary: Repository Docs_

- [x] 1.2 Add OSS contribution and license-facing files
  - Add a license file suitable for an OSS documentation-first project.
  - Add contribution guidance for proposing changes to policies, rubrics, prompts, examples, and usage docs.
  - The completed contribution guidance makes runtime code, AI API integrations, hosted services, and CLI automation out of MVP scope.
  - _Requirements: 6.3, 6.4, 7.1, 7.2, 7.3, 7.4, 7.5_
  - _Boundary: Repository Docs_

- [x] 1.3 Create explicit non-goals and portability documentation
  - Create a dedicated non-goals document that states compact-guard does not perform compaction, replace built-in features, require AI APIs, provide a hosted service, or depend on one vendor or model.
  - Include misuse guidance that redirects users from treating compact-guard as a compaction engine to applying it around an existing compaction workflow.
  - The completed non-goals document is linked from the entry point and tool usage docs.
  - _Requirements: 1.5, 2.5, 5.5, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6_
  - _Boundary: Non Goals Docs_

- [x] 2. Core policy and rubric documents
- [x] 2.1 Create the compaction policy document
  - Define what compacted summaries must preserve, may summarize, and may drop.
  - Include required preservation categories: user intent, current task status, constraints, decisions, unresolved questions, risks, modified files, commands run, verification results, and pending next steps.
  - Explain why each must-preserve category matters for safe resume.
  - The completed policy lets a user distinguish compaction-critical context from low-value conversational detail without relying on a specific vendor or model.
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5_
  - _Boundary: Policy Docs_

- [x] 2.2 Create the summary evaluation rubric
  - Define observable pass, repair-needed, and unsafe-to-resume outcomes.
  - Cover evaluation dimensions for intent, scope, state, constraints, evidence, risks, and next actions.
  - Include criteria that classify missing required context as repair-needed or unsafe-to-resume.
  - The completed rubric can be applied manually from Markdown without AI APIs, hosted services, or vendor-specific evaluators.
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5_
  - _Boundary: Rubric Docs_

- [x] 3. Prompt pack
- [x] 3.1 Create the pre-compact prompt
  - Write a reusable prompt that asks the active agent to prepare preservation-critical session context before the user invokes an existing compact or compress feature.
  - Align prompt language with the policy categories and avoid implying that compact-guard performs compaction.
  - The completed prompt is copy-ready and points users toward the existing tool as the compaction engine.
  - _Depends: 2.1_
  - _Requirements: 3.1, 3.5_
  - _Boundary: Prompt Pack_

- [x] 3.2 (P) Create the post-compact evaluation prompt
  - Write a reusable prompt that evaluates a compacted summary against the rubric outcomes and evaluation dimensions.
  - Include instructions to surface omissions, ambiguity, and unsafe resume conditions.
  - The completed prompt produces an evaluation result that can be mapped to pass, repair-needed, or unsafe-to-resume.
  - _Depends: 2.2_
  - _Requirements: 3.2, 3.5_
  - _Boundary: Prompt Pack_

- [x] 3.3 (P) Create the repair prompt
  - Write a reusable prompt that repairs a failed compacted summary by restoring missing policy-critical context.
  - Include instructions to preserve constraints, unresolved risks, verification evidence, and next steps.
  - The completed prompt asks for a safer summary without introducing claims that are unsupported by the available session context.
  - _Depends: 2.1, 2.2_
  - _Requirements: 3.3, 3.5_
  - _Boundary: Prompt Pack_

- [x] 3.4 (P) Create the resume prompt
  - Write a reusable prompt that guides a resumed agent to continue from an accepted or repaired summary.
  - Include instructions to respect preserved constraints, unresolved risks, and pending next steps before taking action.
  - The completed prompt makes resume behavior cautious and reviewable without depending on one model or vendor.
  - _Depends: 2.1, 2.2_
  - _Requirements: 3.4, 3.5_
  - _Boundary: Prompt Pack_

- [x] 4. Examples and repair walkthrough
- [x] 4.1 Create the bad compacted-summary example
  - Provide an insufficient compacted summary for an AI coding session.
  - Annotate violations such as missing intent, missing implementation state, missing verification evidence, or missing unresolved risk.
  - The completed example clearly maps each flaw to policy or rubric criteria.
  - _Depends: 2.1, 2.2_
  - _Requirements: 4.1, 4.2, 4.3, 4.5_
  - _Boundary: Examples_

- [x] 4.2 (P) Create the good compacted-summary example
  - Provide an acceptable compacted summary for an AI coding session.
  - Annotate which policy and rubric criteria the summary satisfies.
  - The completed example remains tool-neutral unless a tool-specific detail is explicitly called out as illustrative only.
  - _Depends: 2.1, 2.2_
  - _Requirements: 4.1, 4.2, 4.5_
  - _Boundary: Examples_

- [x] 4.3 Create the repair walkthrough example
  - Show how the bad summary is transformed into a safer repaired summary.
  - Explain which omissions are fixed and which remaining uncertainties must stay visible.
  - The completed walkthrough demonstrates repair without claiming compact-guard generated or validated the summary automatically.
  - _Depends: 4.1, 4.2, 3.3_
  - _Requirements: 4.2, 4.3, 4.4, 4.5_
  - _Boundary: Examples_

- [x] 5. Tool-specific usage docs
- [x] 5.1 Create the Codex CLI usage guide
  - Explain how to apply pre-compact, post-compact evaluation, repair, and resume prompts around the existing Codex CLI compact workflow.
  - State that compact-guard does not replace or modify the built-in compact feature.
  - The completed guide lets a Codex CLI user manually adopt compact-guard without AI APIs, hosted services, package scripts, or CLI automation.
  - _Depends: 1.3, 3.1, 3.2, 3.3, 3.4_
  - _Requirements: 5.1, 5.4, 5.5, 6.5, 7.2, 7.3, 7.4, 7.6_
  - _Boundary: Usage Docs_

- [x] 5.2 (P) Create the Claude Code usage guide
  - Explain how to apply pre-compact, post-compact evaluation, repair, and resume prompts around the existing Claude Code compact workflow.
  - State that compact-guard does not replace or modify the built-in compact feature.
  - The completed guide lets a Claude Code user manually adopt compact-guard without AI APIs, hosted services, package scripts, or CLI automation.
  - _Depends: 1.3, 3.1, 3.2, 3.3, 3.4_
  - _Requirements: 5.2, 5.4, 5.5, 6.5, 7.2, 7.3, 7.4, 7.6_
  - _Boundary: Usage Docs_

- [x] 5.3 (P) Create the Gemini CLI usage guide
  - Explain how to apply pre-compact, post-compact evaluation, repair, and resume prompts around the existing Gemini CLI compress workflow.
  - State that compact-guard does not replace or modify the built-in compress feature.
  - The completed guide lets a Gemini CLI user manually adopt compact-guard without AI APIs, hosted services, package scripts, or CLI automation.
  - _Depends: 1.3, 3.1, 3.2, 3.3, 3.4_
  - _Requirements: 5.3, 5.4, 5.5, 6.5, 7.2, 7.3, 7.4, 7.6_
  - _Boundary: Usage Docs_

- [x] 6. Integration and quality review
- [x] 6.1 Integrate cross-document links and terminology
  - Ensure the entry point, docs index, policies, rubrics, prompts, examples, usage docs, non-goals, and contribution guidance link to each other consistently.
  - Normalize shared terminology for compacted summary, safe resume, repair-needed, unsafe-to-resume, and existing compact / compress features.
  - The completed documentation set can be followed from README through policy, rubric, prompt, example, and tool-specific adoption paths without dead-end MVP sections.
  - _Depends: 1.1, 1.2, 1.3, 2.1, 2.2, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3, 5.1, 5.2, 5.3_
  - _Requirements: 1.5, 3.5, 4.5, 5.4, 5.5, 6.1, 6.2, 6.3, 6.5, 7.6_
  - _Boundary: Repository Docs, Non Goals Docs, Policy Docs, Rubric Docs, Prompt Pack, Examples, Usage Docs_

- [x] 6.2 Validate requirements coverage and non-goal boundaries
  - Review every Markdown artifact against the requirements and design traceability.
  - Confirm no MVP document introduces runtime code, package scripts, AI API integrations, hosted services, or CLI automation.
  - Confirm the completed documentation covers all required policy, rubric, prompt, example, usage, OSS package, and portability requirements.
  - _Depends: 6.1_
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 2.1, 2.2, 2.3, 2.4, 2.5, 3.1, 3.2, 3.3, 3.4, 3.5, 4.1, 4.2, 4.3, 4.4, 4.5, 5.1, 5.2, 5.3, 5.4, 5.5, 6.1, 6.2, 6.3, 6.4, 6.5, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6_
  - _Boundary: Repository Docs, Non Goals Docs, Policy Docs, Rubric Docs, Prompt Pack, Examples, Usage Docs_
