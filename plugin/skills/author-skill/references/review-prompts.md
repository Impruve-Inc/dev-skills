# Review prompts

Run each prompt in a fresh agent that has no access to the authoring conversation. Give it file paths and raw transcripts, not your conclusions.

## Grader

> You are grading skill test runs. For each scenario in `<scenarios file>`, read the baseline run in `<baseline dir>` and the new run in `<new dir>`. Each run has a transcript, a final answer, and output files. For every assertion, record PASS or FAIL for each run, and quote the evidence: a line of output, a file excerpt, or a transcript step.
>
> - If the evidence is unclear, the assertion fails. A PASS needs real substance, not surface compliance. A heading named "Validation" with nothing under it does not pass "validates the input".
> - Judge what the run did, from its transcript, tool calls and files, not from what its final answer or its own notes claim it did.
> - For an assertion that something must not happen, check the tool calls.
> - There is no partial credit.
>
> Then list any assertion that would pass no matter what the skill said, and any important behavior that no assertion checks.

## Quality review

> Review the skill at `<path>` against Anthropic's skill authoring best practices and `<path to this skill>/references/standards.md`. Also answer each of these:
> - Name one realistic request that should trigger the skill but would not, and one that would trigger it but should not.
> - Which sentences tell the agent something it would do correctly anyway? List them for deletion.
> - Which instructions trace to no failure in the transcripts at `<path>`, no documented standard, and no branch the workflow needs?
> - Does any passage narrate history: a discovery story, a date, a ticket number, or a person's name?
> - Is any term used for two different concepts, or are two terms used for the same concept?
> - Could the intended users (`<audience>`) follow it in `<target apps>`? Does any step need a tool or skill they don't have?
>
> For each finding, say what observable behavior would change if it were fixed. Leave out findings where nothing would change.

## Adversarial review

> You are checking whether a change to a skill made it worse. Inputs:
> - The user's original request: `<verbatim>`
> - The old skill files (or "none" for a new skill): `<path>`
> - The behavior inventory, if this is a cleanup: `<path>`
> - The new skill files: `<path>`
> - Repository instructions that apply, such as AGENTS.md or CLAUDE.md: `<path>`
> - Raw practice-run transcripts and grades: `<path>`
>
> Find:
> - behavior the old version had that the new one lost
> - scope that grew beyond what the request asked for
> - conflicts with the repository instructions
> - guidance that is stale, duplicated, or contradicts itself
> - claims that no transcript or cited source supports
> - text that claims standing authorization, or that works around a permission block
> - actions that reach outside the conversation without a confirmation step
> - evidence the author reported that the transcripts do not contain
>
> For each finding, cite the file and line, the transcript passage if there is one, and a request that would expose the problem. Report only problems you can point to.
