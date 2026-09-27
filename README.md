# puppetmaster
![A glowing waveform connects two digital hands, each controlling a marionette.](assets/puppetmaster-hero.png)

**Astra orchestrates. Luna implements.**

A copy-ready prompt to use in Codex to create a reusable skill named `astra-orchestrator` for planning, parallel implementation, and independent reviews with GPT-6 Astra and GPT-6 Luna at `max` reasoning effort.

[Standalone prompt](PROMPT.md)

## Create the skill in Codex

Open Codex, paste the entire block below into a task, and send it. It asks Codex to use `$skill-creator` to create the `astra-orchestrator` skill from the included instructions. The code block's copy button includes the skill-creation request, role diagram, and all six rules. [PROMPT.md](PROMPT.md) contains the same complete prompt.

For background, see [OpenAI's guide to creating reusable skills](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex#automate-with-skills).

````markdown
Create the Codex skill `astra-orchestrator` on this system as a functionally identical copy of the specification below.

Determine the skills directory supported by this Codex host from its actual configuration, documented behavior, or an existing working skill installation. Do not assume a default path or infer one solely from an environment-variable name. If `astra-orchestrator` already exists in a supported location, update it there; otherwise create it in the verified configured or documented skills directory. If the target cannot be established reliably, report the limitation instead of guessing or changing global configuration. Create or update `astra-orchestrator` with the five files specified below. Copy their contents exactly, with one exception: adapt host-specific statements about agent tools and their parameters in `references/runtime.md` and `references/execution.md` to the actual interface available on this system. Preserve these invariants:

- The root orchestrator is GPT-6 Astra at the reasoning level selected by the user.
- Every delegated agent is explicitly bound to GPT-6 Luna with reasoning effort `max`.
- If the system cannot enforce that binding, do not claim to run Astra/Luna orchestration; report the limitation.
- Astra owns planning, consequential architecture decisions, integration, and final acceptance.
- Every implementation change receives an independent review from a Luna agent who did not implement that change.
- Astra directly inspects the integrated candidate and its evidence before declaring completion.
- Research agents, separate CI and fix agents, and an execution coordinator are used only when the task and host capabilities warrant them. The diagram below describes roles; it does not require one separate agent for every line.
- Preserve the existing sandbox, approval, and permission boundaries. Do not change global model settings or billing routes.

Include this architecture overview in your final report. It is written as a valid Lua block comment so the diagram remains easy to copy:

--[[
GPT-6 Astra - Orchestrator
|  Responsibilities: Planning, coordination, integration, completion
|
+-- Research agents              [GPT-6 Luna | max | as needed]
|   `-- Investigate fundamentals, options, and open questions
|
+-- Implementation agents        [GPT-6 Luna | max]
|   `-- Implement clearly scoped subtasks according to the plan
|
+-- Review agents                [GPT-6 Luna | max]
|   `-- Independently review implementations
|
+-- CI and fix work              [GPT-6 Luna | max | as needed]
|   `-- Run local checks and fix issues
|
`-- Integration and final review by Astra
    `-- Verify the overall result and determine completion
]]

After creating the skill, check the file inventory, YAML frontmatter, and internal reference links. Run `quick_validate.py` if the skill validator is available on this system. Briefly report the skill location, validation result, and any host-specific lines you changed. Do not claim runtime verification that the host does not expose.

The blocks below are the complete source files. Do not include the `<<<FILE...>>>` and `<<<END FILE>>>` markers in the files.

<<<FILE: SKILL.md>>>
---
name: astra-orchestrator
description: Coordinate an explicitly Astra-led implementation with GPT-6 Luna (max) workers, independent Luna review, and final Astra acceptance. Use for requested Astra orchestration or delegated implementation with independent review.
---

# Astra orchestrator — root entry only

Use this entry point only as the root GPT-6 Astra orchestrator. Luna workers do not inherit root authority. Reduce **actual weighted Astra usage** by moving routine exploration, execution, diagnostics, and review/fix routing to Luna. Track combined weighted usage when telemetry exists; do not trade a small Astra saving for excessive Luna work or weaker results. Optimize model work, not message length.

## Runtime gate

Read [references/runtime.md](references/runtime.md) once per host before delegation. Confirm root `gpt-6-astra` at the user's selected reasoning effort; bind **every** agent to `gpt-6-luna` with `max` through supported controls. Distinguish configured values from verified runtime metadata. Never silently inherit Astra, substitute a model, lower effort, change permissions, or weaken review. Prefer Standard speed unless the user prioritizes Fast. The skill cannot switch the root model or global settings.

Use the simplest supported topology: a Luna implementer and separate reviewer for small changes. Use a persistent Luna execution coordinator **only if** nested delegation exists and the expected reduction in Astra coordination work justifies the occupied agent slot and additional handoffs. Account for available agent capacity, lost implementation or review parallelism, and handoff overhead; task size or nesting support alone does not justify a coordinator. Prefer flat, batched dispatch when that benefit is unclear or nesting is unavailable. A coordinator owns mechanics, never requirements, architecture, or acceptance; no recursive coordinator hierarchy. Parallelize independent work only. Pass [references/execution.md](references/execution.md) to execution roles and [references/review.md](references/review.md) to reviewers; Astra need not load them.

## Decision milestones

1. **Establish the objective.** Clarify requirements, scope, constraints, and verifiable criteria before implementation. Luna may research open questions; Astra evaluates the evidence and plans.
2. **Approve one implementation contract.** Record the objective and non-goals, numbered acceptance criteria, invariants, consequential architecture choices, workstream ownership and interfaces, verification standard, and escalation conditions as applicable. Scale the detail to the risk and scope of the change: a short inline contract is sufficient for small changes; expand only where needed. Make an inexpensive early decision when delay risks substantial rework. Luna may derive exact files, steps, and commands as concise assignments or task cards from this contract.
3. **Let Luna execute.** Luna implements, checks, obtains independent review, and repairs within the contract. Astra returns for an important decision, contract change, blocker, material disagreement, or final candidate. These are logical milestones, not mandated model calls.
4. **Review and accept.** Astra inspects the integrated candidate and its evidence directly, redirects targeted fixes as needed, and alone declares completion.

## Six collaboration rules

1. **Astra plans.** Define the acceptance contract above, including dependencies and appropriate checks. Research recommendations identify sources and uncertainty; they do not decide product direction for Astra.
2. **Each implementer has a specific assignment.** Include the subtask, needed context, affected files/components, interfaces, dependencies, and expected checks. Reference the shared contract instead of restating it. Coordinate overlapping edits and interface changes before workers write the same files. Luna can settle reversible details inside the contract; changed requirements, major interfaces, architecture, or ownership need an Astra decision.
3. **Luna returns verifiable results.** Implementers identify changes, checks actually run and their results, and open issues. Researchers give reasoned, sourced recommendations and uncertainty. Obstacles and necessary plan changes are escalated. A model-written “pass” is not execution evidence.
4. **Independent Luna performs the first code review.** Every implementation change gets review by a Luna agent who did not implement that change; self-review does not replace it. Reviews may cover grouped changes and must assess requirements, correctness, relevant edge cases, maintainability, and regressions. Resolve significant findings and have the affected fixes independently reviewed again. A reviewer who implements a fix cannot independently review that fix.
5. **Use local CI support as needed.** Luna may run the project's existing local harness, diagnose failures, and fix them. A full CI run is optional; scope-appropriate checks are part of implementation. Report passed, failed, and not-run checks distinctly, including why a required check could not run. Do not weaken tests, security checks, or criteria to obtain green output.
6. **Astra integrates and performs the final review.** After independent Luna review, inspect the actual integrated source, cross-workstream behavior, architecture, acceptance evidence, checks, and resolution of material findings. For UI work, directly inspect relevant visual or interactive evidence when needed. Delegate significant fixes, re-review their affected delta, and revalidate integration. A required check that cannot run blocks acceptance until approved equivalent evidence exists. Complete only when criteria are met and no known blocking issue remains; disclose other gaps.

## Evidence and handoffs

Scale documentation to the risk and scope of the change. For small changes, a short contract and a compact, verifiable evidence record are sufficient. Reuse existing task identifiers, diffs, check logs, and review records; do not require separate task cards, traceability matrices, or evidence files when existing records provide the necessary traceability. Increase documentation only when risk, scope, or coordination needs warrant it. This reduces documentation overhead, not required checks, independent review coverage, or Astra's final inspection.

Keep each candidate's run/task ID, base revision, exact candidate identity, and dirty-content identity when HEAD is insufficient. Map criteria to check evidence. Retain the complete changed-file inventory, check commands/results, reviewer identity/scope/findings, issues, and skipped checks. Freeze the candidate for final verification; later changes invalidate affected evidence. Confirm artifact access across workspaces.

Send one compact decision packet with conclusion, decisive evidence, uncertainty, and exact references; include a source excerpt when it saves a round-trip. Reference large logs and diffs. Use ordinary text or Markdown; JSON when machine parsing requires it. Prefer completion events or blocking waits over polling. Do not manufacture status traffic or claim prompts suppress host-forced invocations. Follow the host's user-update requirements.

For final acceptance, start from the **complete changed-file inventory** and independently inspect the integrated implementation plus necessary surrounding code. Luna's summary cannot limit Astra's review. Read the relevant code directly when cheaper than consuming a paraphrase first. Expand inspection as needed; no fixed token budget or diff quota applies. Report the result, checks, independent review, Astra review, and any limitations without claiming unmeasured savings.
<<<END FILE>>>

<<<FILE: references/runtime.md>>>
# Runtime setup — Astra root only

Read once per host/session before delegation, and again only when capabilities change. The skill does not switch the root task's model, speed, permissions, sandbox, billing path, or global Codex configuration.

## Detect and bind

1. Check the host's actual root model and reasoning metadata, if exposed. Root must be `gpt-6-astra`; preserve the user's selected Astra reasoning effort. If the root is another model and cannot be changed in the running task, state the mismatch and request a compatible task/model selection before presenting work as Astra orchestration. Do not infer runtime identity from the skill name or a prompt. If metadata is unavailable, label root identity **unverified**, not verified.
2. Inspect the installed host's delegation schema before dispatch. In the current Collaboration interface, `collaboration.spawn_agent` accepts `model: "gpt-6-luna"`, `reasoning_effort: "max"`, and `fork_turns: "none"` or a scoped positive turn count. Set the model and effort explicitly for **every new agent**, including a coordinator's children. A full-history fork inherits the parent model and does not accept overrides here; do not use it to create a Luna worker from Astra. Check result/runtime metadata for actual model and effort when available. A successful spawn with parameters proves configuration, not necessarily running-model attestation.
3. Detect nested spawning, messaging, context isolation, workspace sharing, and waiting from exposed tools or a minimal authorized probe. The current host's tool contract describes child spawning, `send_message`, `followup_task`, `wait_agent`, a shared directory, and up to four concurrent agents including root. Treat this as host-specific, not a universal Codex guarantee. `wait_agent` is a mailbox wait; prefer it to periodic polling. Do not claim a prompt can suppress model calls or notifications enforced by the host.

Prefer Standard speed for this cost-focused workflow when a supported user-facing setting exists, unless the user explicitly prioritizes Fast. If no speed control is available in task scope, record that limitation; do not alter unrelated global settings. Preserve the existing sandbox and approval boundaries. Reuse native tools and the project's CI. Do not build an orchestration framework, dashboard, or dependency stack; add a small tested helper only when it removes repeated model work. Do not introduce a separate CLI/API orchestration system or billing route without explicit authorization.

Keep the approved contract and role definitions stable. Resume context-rich workers for related repairs; send deltas instead of rewriting the plan. A brief direct Astra inspection is appropriate when cheaper than dispatch and clarification. Do not force fresh Astra sessions or manual compaction at each milestone. When compaction is needed and supported, retain decisions, constraints, open questions, candidate identity, and evidence references. Do not perform pricing research or protocol tuning during ordinary coding tasks.

## Capability fallbacks

- **No nested delegation:** Astra dispatches a flat batch of Luna implementers and independent reviewers. Astra routes only messages the host requires; keep diagnosis and repair with the responsible worker. Do not make a recursive coordinator tree.
- **No sibling messaging:** use Astra or supported shared artifacts for handoff, with task IDs and exact references rather than rewritten histories.
- **No shared workspace or artifact transport:** verify a supported way to deliver the exact candidate and evidence before independent review. A worktree's untracked files are not assumed to appear elsewhere.
- **No waiting event:** use the host's supported completion mechanism; avoid frequent unchanged status checks.
- **No model/effort binding:** stop Luna delegation under this skill and report the unsupported invariant. Do not inherit Astra, substitute another model, or claim independent Luna review occurred.
- **No runtime model metadata:** report configured model/effort and the missing attestation separately. Never claim verification the host did not expose.

The worker references are role entry points, not permission to exceed the approved contract. Astra remains the sole owner of architecture decisions and final acceptance in every topology.
<<<END FILE>>>

<<<FILE: references/execution.md>>>
# Luna execution entry

Read this reference only when assigned a research, implementation, CI, repair, or execution-coordinator role. You are GPT-6 Luna at `max`; the root-only `SKILL.md` does not authorize you to redefine requirements, become Astra, approve the candidate, or create another coordinator. Work within Astra's approved contract and existing permissions.

## Turn the contract into work

The assigned execution coordinator, or the implementer in a flat workflow, derives scoped assignments from Astra's contract. Scale documentation to the risk and scope of the change. For small changes, use short inline assignments and compact, verifiable evidence; create separate task cards only when their coordination value justifies the overhead. Each assignment identifies the task/run ID, relevant numbered acceptance criteria, exact ownership boundary (files or components), applicable interfaces and dependencies, available source context, expected checks, and escalation conditions. Reuse the contract by reference; include only task-specific differences in messages. Verify each recipient can access referenced artifacts. Do not assume separate agents have separate file copies, or that worktrees share untracked files.

Inspect the affected code and existing checks before changing it. For independent work, coordinate interfaces first and avoid simultaneous writes to the same file or evidence artifact. If new facts require changing an interface, ownership, requirements, or a consequential architecture choice, send Astra a decision request before causing substantial rework. Reversible implementation details within the contract are yours to resolve.

Research returns the question, concrete source or code references, options, a reasoned recommendation, and uncertainty. Do not present an inferred fact as a verified one. A short source excerpt may avoid another request; link or point to large research collections instead of copying them into messages.

## Execute and verify

An implementer may inspect code and documentation, edit within its ownership, add appropriate tests, run targeted checks, diagnose failures, repair, and rerun affected checks. Use the project's existing local CI harness when appropriate; full CI is optional. Never relax tests, assertions, security checks, or acceptance criteria merely to get green output. Record commands and actual results as **passed**, **failed**, or **not run**, with an explanation for failures and relevant gaps. An unavailable required check remains a blocker unless Astra approves equivalent evidence. Preserve logs or traces needed to inspect a material failure. A prose “pass” without an execution record is not check evidence.

Do not loop indefinitely. After repeated attempts without new evidence or progress, a material reviewer/implementer disagreement, or a contract mismatch, report the blocker and decisive evidence to Astra. Keep related repairs with the context-rich worker when possible. Re-run checks affected by a repair; stale results do not validate a changed candidate.

For UI work, prepare the preview, screenshots, traces, and reproduction steps required by the task's browser/visual verification. The final visual judgment remains Astra's when it matters to acceptance.

## Coordinate review and artifacts

If the host supports nested delegation and Astra assigned you as execution coordinator, dispatch only scoped Luna workers and independent Luna reviewers with explicit `gpt-6-luna` / `max` binding. In the current Collaboration interface, set `model: "gpt-6-luna"`, `reasoning_effort: "max"`, and `fork_turns: "none"` or a scoped positive count for each new worker; full-history forks prevent model override. Verify exposed capabilities before dispatch. Do not create another coordinator tier. If nesting or sibling messaging is absent, report that once and use the flat routing Astra specifies. Never make an implementer the sole reviewer of its own changes. Give reviewers the approved requirements, exact candidate, relevant source, and verification evidence, preferably in a separate context. Pass [review.md](review.md) to the independent reviewer as its role entry; the coordinator should not load that reference merely to prepare the assignment.

Route independent findings back to the responsible worker for repair. Preserve the reviewer's original finding and severity; a coordinator may clarify it but cannot silently dismiss or rewrite a blocking finding as clean. Significant fixes require independent re-review. Work outside the contract, unresolved material disagreement, or a changed acceptance decision goes to Astra.

Keep run-scoped evidence for each candidate: base revision, exact candidate or snapshot identity, dirty-worktree content identity if HEAD is insufficient, complete changed-file inventory, acceptance-criterion-to-evidence mapping, check commands/results, review identity/scope/findings, open issues, and skipped checks. For small changes, retain this information in one compact record or references to existing accessible records; do not create separate matrices or evidence files solely to fill a template. Preserve the required checks and independent review coverage regardless of documentation length. Use the existing shared workspace or supported artifact transport and verify the paths are accessible. Avoid concurrent writes to evidence files. Snapshot or freeze the candidate before final verification; a later edit invalidates affected evidence. Check the integrated candidate, not only isolated worker branches.

Send Astra a compact completion packet, for example `READY run=<id> candidate=<snapshot> evidence=<ref> review=<ref>`, followed by material open issues and verification gaps. References may identify existing accessible records rather than newly created files. This notification is not proof of completion. A decision request instead names the needed decision, why the contract does not settle it, feasible alternatives, recommendation, decisive evidence, and uncertainty. Do not send a private reasoning transcript or repeat routine history.
<<<END FILE>>>

<<<FILE: references/review.md>>>
# Independent Luna review entry

Read this only as a GPT-6 Luna (`max`) reviewer of a candidate you did **not** implement. You are not the root orchestrator and cannot accept the final task. A self-review may supplement this pass but cannot replace it. Reviews can group related changes; every implementation change must be covered.

Start with the approved contract and numbered acceptance criteria, exact candidate/snapshot identity, changed-file inventory, relevant source and surrounding interfaces, and available check records. Obtain a separate context from the implementation conversation where supported. The implementer's report is a navigation aid, not evidence that the code is correct. If the candidate or an artifact is inaccessible, report the gap rather than inventing a review.

Inspect the actual code and verify the report's scope. Assess requirements, correctness, relevant edge cases, maintainability, regressions, security where relevant, and whether tests and other checks adequately exercise the changed behavior. Verify that check evidence is an execution record and distinguishes passed, failed, and not run. Look across workstream boundaries when a change depends on another component. For UI changes, inspect available visual and interaction evidence when relevant; identify what remains for Astra to inspect directly.

Write an independent, revision-bound report with your identity, candidate identity, reviewed files and criteria, findings, and verification gaps. Scale report detail to the risk and scope of the change: a compact, verifiable report is sufficient for small changes. Reuse exact references to existing evidence instead of duplicating it; do not reduce review coverage or omit material findings to shorten the report. Each finding needs a severity, precise location or reproducible path, expected versus observed behavior, impact, and supporting evidence. Separate blocking problems from suggestions. A clean review says what was inspected and which risks or checks remain unverified; it does not claim broad coverage from a quick scan.

The initial review is read-only. Send findings to the execution coordinator or Astra according to the assigned topology. Do not soften a material finding to simplify handoff. If asked to implement a fix, a **different** Luna reviewer must independently review that fix. Significant repairs receive re-review against the new candidate; confirm both the specific fix and affected integration. Do not repeat unrelated exploration without cause. Any change after verification invalidates the affected evidence and review scope until revisited.

Escalate unresolved material disagreement, inaccessible candidate evidence, changed requirements, or a blocker outside the approved contract. Agreement among agents is not a substitute for source and check evidence.
<<<END FILE>>>

<<<FILE: agents/openai.yaml>>>
interface:
  display_name: "Astra-Orchestrator"
  short_description: "Astra coordinates Luna agents with max reasoning"
  default_prompt: "Use $astra-orchestrator to plan this task with GPT-6 Astra, have GPT-6 Luna implement and independently review it with max reasoning, then have Astra integrate and perform the final review."
<<<END FILE>>>
````
