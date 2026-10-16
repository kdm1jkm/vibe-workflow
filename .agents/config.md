# Workflow configuration

Template defaults, not evidence of target-repository policy. Initialization MUST replace detection fields and confirm unresolved policies. This is human-readable Markdown, not a required machine schema. Paths are repository-relative unless linked otherwise.

## State labels (fixed)

| Label | Meaning |
| --- | --- |
| `agent:working` | Claimed by an active agent session |
| `agent:passed` | Objective checks and independent agent quality review passed; PR awaits human merge |
| `agent:blocked` | Requires human decision or external action |

## Priority labels

Descending: `priority:high`, `priority:medium`, `priority:low`, then unlabeled. If multiple priority labels exist, use the highest. FIFO (`created_at` ascending, number ascending as tie-break) applies within a tier.

## PR merge strategy

Default: `squash`. Alternatives: `rebase` (linear individual commits), `merge` (preserve branch topology). Humans merge; PASS does not authorize an agent merge.

## Bounded self-repair limit

`K = 3` repair iterations per session across local failures, CI failures, and review findings. Initial verification is not a repair. Do not reset the budget on tool, skill, or phase transitions. At exhaustion record evidence, release the claim, hand off, and stop.

## Language policy

Default: Korean (`ko`) for human-facing output.

| Output | Language |
| --- | --- |
| Replies to the user, questions, reports | Korean |
| Issue/PR comments, Canonical Spec Snapshot content, PR title/body | Korean |
| `.agents/tasks/<issue-number>.md` content | Korean |
| Commit messages | Follow the target repository convention; Korean if none is detected and the user did not choose otherwise |

Fixed regardless of language: snapshot and task-file Markdown headings defined in the [workflow model](reference/workflow-model.md) (for example `## Canonical Spec Snapshot`, `### Acceptance Criteria`), label names, branch names, file paths, commands, code identifiers, and quoted logs/errors. Agents locate artifacts by these exact headings; translating them breaks discovery. Skill contracts stay in English for stable agent interpretation.

Initialization MAY propose a different default when the target repository's existing Issues/PRs/commits consistently use another language; the user's choice is recorded here.

## Task file retention policy

Default: `keep-in-git`.

- `keep-in-git`: commit `.agents/tasks/<issue-number>.md` for portable handoff.
- `clean-before-pass`: update task state, promote durable decisions to project docs or the PR, then remove the temporary task file before final verification and PASS. Retain it on incomplete handoff.
- `gitignored-local`: ignore only numeric task Markdown files; preserve the directory marker. Before handoff, mirror essential context in an Issue/PR comment because local files do not cross worktrees or machines.

## Verification commands

| Check | Target command / applicability |
| --- | --- |
| test | Not detected: inspect target manifests and CI during initialization |
| lint | Not detected: inspect target manifests and CI during initialization |
| typecheck | Not detected: inspect target manifests and CI during initialization |
| build | Not detected: inspect target manifests and CI during initialization |

Record exact commands, working directories, prerequisites, and required CI checks from package manifests and CI. For an absent check explicitly record `Not applicable` with a reason; an unknown or unavailable required check is not a pass. This template contains Markdown instructions, not an application test/build stack. Verify it by link/frontmatter validation and scenario walkthroughs.

## MCP extensions

Approved project MCPs: none. Standard file tools, Git, and GitHub CLI/native tools are sufficient by default. Suggest extensions only for a demonstrated missing capability; explain access, credentials, permissions, and trade-offs. Do not install or edit global MCP configuration without approval.

## Initialization commit policy

Unresolved in targets without a convention: ask whether to leave changes unstaged, stage selected changes, or create an initialization commit. Never commit unrelated user files.

## References

- [Workflow model](reference/workflow-model.md)
- [Compatibility](reference/compatibility.md)
