# Testing to Impruve's standards

These add to skill-creator's testing loop. They don't replace it.

## Contents
- Use the repository's evals first
- Users' conditions
- Running test cases in Claude Code
- Skills that act on other systems
- Skills that produce files
- Without skill-creator

## Use the repository's evals first

If the repository already has eval cases or an eval runner, start your test cases from them and run them its way. Read its testing docs first: they often record traps, such as how to mock a server or why a comparison measures nothing.

## Users' conditions

- **Models.** Use the models the users have, and run at least once on the smallest. Compare matching pairs: the baseline and the new version on the same model, with the same prompt, files and tools. Subagents may only offer model families, such as sonnet or haiku, not the exact version the repository pins. Record the model each run used, and note any difference from the pinned one.
- **Tools.** Withhold what the users' app lacks, such as a renderer or network access.
- **The app itself.** For a skill people use in the Claude app (chat, Desktop or Cowork), finish with one short smoke test there. Upload the skill, send a realistic request without naming the skill, and check that it loads. Only the app shows which tool it reaches for. It may route a request to its own tools, such as Claude Slides, before any skill.
- **Slash commands.** A prompt like `/plugin:skill` fails with "Unknown command" when the skill is absent, so a no-skill baseline of it measures nothing. Give both sides the request in plain words.

## Running test cases in Claude Code

- **The baseline can't see the skill.** A fresh subagent can still see installed skills. Run the baseline where the skill isn't installed, or point the subagent at a scratch copy of the skill folder, and check its transcript to confirm what it loaded.
- **Real evidence.** Keep each run's output files and transcript outside the repository. A run's summary of what it did is a claim. Claude Code saves each subagent's transcript under `~/.claude/projects/<project>/<session>/subagents/agent-<id>.jsonl`.
- **When the repository's eval runner can't run.** Serve mocked tools through something that logs each call, such as a small script the subagent must call, and grade the log. A subagent told to read mock files directly bypasses the log, and then nothing shows which calls it made.
- **API errors.** A run that stops on an API error says nothing about the skill. Retry it once, then switch both sides of the pair to another model and note it.

## Skills that act on other systems

When the skill moves tickets, sends messages, merges code or calls an MCP server, test runs must not touch the real system.

- Serve the server's tools from mocks with the real tool schemas, and grade the mocked calls.
- Deny commands that reach real remotes, such as `git push`, `gh pr create` and `gh repo create`.
- An assertion that something must not happen ("no ticket moved", "nothing edited before approval") is graded on the tool calls, not the final answer.

## Skills that produce files

- The grader checks the file itself, not the agent's description of it. Render each page and look at it, where a renderer exists.
- Give the skill a checker script for what can be detected without rendering. Calibrate it on the real baseline and new outputs: every ERROR should be a real defect. Add each known failure as a test of the checker in the repository's CI, so the checker can't silently stop catching it.
- Grade whether the delivery message is accurate. An agent that says "nothing changed" when it hid text has failed, even if the file is otherwise fine.

## Without skill-creator

If skill-creator isn't available:

1. Write three test cases from real tasks. Each one is a prompt, its input files, and three to six checkable assertions.
2. Run each test case in a fresh agent without the change, then with it, under the users' conditions.
3. Have a separate fresh agent grade both sides with the grader prompt in the review prompts reference.
4. Fix the cause of each failure and re-run the affected test case.

For trigger testing, write five prompts that should activate the skill and five near misses. Send each in a fresh session three times. A should-trigger prompt passes at two of three, and a near miss passes at one or fewer.
