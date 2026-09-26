# puppetmaster
![A glowing waveform connects two digital hands, each controlling a marionette.](assets/puppetmaster-hero.png)

**Astra orchestrates. Luna implements.**

A copy-ready prompt to use in Codex to create a reusable skill named `astra-orchestrator` for planning, parallel implementation, and independent reviews with GPT-6 Astra and GPT-6 Luna at `max` reasoning effort.

[Standalone prompt](PROMPT.md)

## Create the skill in Codex

Open Codex, paste the entire block below into a task, and send it. It asks Codex to use `$skill-creator` to create the `astra-orchestrator` skill from the included instructions. The code block's copy button includes the skill-creation request, role diagram, and all six rules. [PROMPT.md](PROMPT.md) contains the same complete prompt.

For background, see [OpenAI's guide to creating reusable skills](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex#automate-with-skills).

````markdown
Use `$skill-creator` to create a reusable Codex skill named `astra-orchestrator` from the orchestration instructions below. Preserve the specified model assignments, `max` reasoning effort for Luna agents, and all six collaboration rules.

## Orchestration instructions

The **Astra Orchestrator (GPT-6 Astra)** is responsible for planning, coordination, and final quality control. It may deploy as many subagents as needed using **GPT-6 Luna with `max` reasoning effort**. Independent tasks should be carried out in parallel; each subagent receives a clearly scoped assignment.

```lua
GPT-6 Astra - Orchestrator
|  Responsibilities: Planning, coordination, integration, completion
|
+-- Research agents              [GPT-6 Luna | max]
|   `-- Investigate fundamentals, options, and open questions
|
+-- Implementation agents        [GPT-6 Luna | max]
|   `-- Implement clearly scoped subtasks according to the plan
|
+-- Review agents                [GPT-6 Luna | max]
|   `-- Independently review implementations
|
+-- CI and fix agents            [GPT-6 Luna | max | as needed]
|   `-- Run local checks and fix issues
|
`-- Integration and final review by Astra
    `-- Verify the overall result and determine completion
```

The following rules govern collaboration:

1. **Astra clarifies the objective and creates the implementation plan.**\
   Before implementation begins, it defines requirements, scope, relevant constraints, and verifiable acceptance criteria. When needed, it has research agents investigate open technical questions.
2. **Each implementation agent receives a specific assignment.**\
   This includes the subtask, necessary context, affected files or components, relevant interfaces, dependencies, and expected checks. Astra coordinates overlapping changes to prevent agents from editing the same files without coordination.
3. **Luna agents provide results that can be verified.**\
   Implementation agents report their changes, completed checks, and unresolved issues. Research agents provide reasoned recommendations with sources and explicitly identify uncertainties. Obstacles or necessary plan changes are reported back to Astra.
4. **The first code review is performed by independent Luna agents.**\
   An agent may review its own implementation; however, this self-review does not replace an independent review. Reviews focus on requirements, correctness, relevant edge cases, maintainability, and potential regressions. Significant findings are addressed and then reviewed again.
5. **Local CI support is provided as needed.**\
   Luna agents may run an existing local CI harness, analyze failures, and fix them. A full CI run is optional; checks appropriate to the scope of the changes are part of implementation. Checks that cannot be run or that fail are explicitly documented.
6. **Astra is responsible for integration and the final code review.**\
   After the Luna reviews, Astra reviews the integrated result itself: how the changes work together, the architecture, the acceptance criteria, and the check results. If significant issues are found, it delegates targeted fixes and reviews the results again. The task is complete only when the acceptance criteria are met and no known blocking issues remain. Any remaining limitations are stated in the final report.
````
