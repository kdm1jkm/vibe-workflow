# Repository-native Agentic Development Workflow

A reusable initialization template for Codex, Claude Code, and OMP / oh-my-pi. It installs a repo-local, GitHub-centered workflow: Issue → progressive spec → TDD implementation → PR → self-review → independent review → PASS → human merge.

The workflow consists of Markdown contracts and standard Git/GitHub interfaces. No custom orchestrator, daemon, agent adapter, global configuration, or ongoing synchronization with this template is required. After initialization the target repository owns its policies and skills.

## Quick start

Open your coding agent **in the target repository**. Paste the prompt below and supply this template repository's actual clone URL or an existing local checkout path on the first line. This template does not invent a publication URL; a local source checkout works before it is published. Use the template URL, not the target URL.

```text
Template source: <actual template Git clone URL or absolute local checkout path>

Initialize a repository-native Agentic Development Workflow in the current
TARGET repository using the template source above. Do not clone over this repo.
If the source is a URL, clone it to a separate temporary directory with
`git clone <template-source> <temporary-directory>`; otherwise inspect the local
source checkout. Read the source AGENTS.md, then its linked
.agents/skills/init-workflow/SKILL.md and all referenced contracts. Execute that
skill using Minimal Core + Auto-detect and Selective Merge.

Inspect the target's stack/manifests, docs, AGENTS.md, CLAUDE.md, .agents/,
.claude/, .omp/, CI workflows, hooks, Git/worktrees, labels, merge settings and
commit conventions. Detect exact existing test/lint/typecheck/build commands
and required CI checks; do not invent a toolchain. Preserve project instructions,
CI, hooks, user changes and secrets. Do not copy the template README or Git
history into the target.

Batch unresolved policy decisions with options/trade-offs, inheriting choices
already unambiguously defined in the target:
1. Priority labels: descriptive priority:high/medium/low vs existing P0/P1/P2.
2. Human PR merge strategy: squash vs rebase vs merge, within repo permissions.
3. MCPs: none by default; propose only demonstrated essential capabilities,
   explaining permissions, credentials and CLI alternatives before installation.
4. Task retention: keep-in-git for portability; clean-before-pass for less diff
   noise after durable context promotion; gitignored-local with GitHub handoff
   mirrors because local files do not cross worktrees.
5. Exact conflicts with existing agent instructions/skills/CI/hooks: preserve and
   selectively merge by default; obtain approval before replacement or policy changes.
6. Initialization commit policy: leave unstaged, stage selected files, or create
   an atomic commit under existing/agreed conventions; exclude unrelated changes.
Await required choices. Do not silently accept template defaults as consent.

Install the flat Task/Capability Skills and workflow references in .agents/;
write target config from approved/inherited policies and detected commands;
selectively merge a minimal AGENTS.md index; create or deduplicate @AGENTS.md
in CLAUDE.md without replacing project instructions. Preserve existing CI/hooks.
Ensure agent:working, agent:passed, agent:blocked and chosen priority labels exist
on the GitHub remote when authorized and accessible. Assess MCP capability first;
no global settings changes or unapproved MCP installation.

Validate all local links, skill frontmatter/discovery and Claude import. Exercise
installed status/selection read-only with live data when available, or clearly
labeled fixtures; verify the user-selection pause. Check available agent discovery
and document unavailable agents. Run applicable target verification, inspect Git
status and honor the chosen commit policy. Report actual checks, merged/preserved
files, confirmed policies and any remote-access prerequisite. Clean up only your
temporary checkout. STOP; do not automatically start an Issue or merge a PR.
```

For an empty **new** repository, you may instead use this repository as a GitHub template, then run the initialization skill against the resulting repository to detect its actual policies and stack. Do not treat the supplied config defaults as a completed target initialization.

### Requirements and access

- Git and authenticated GitHub CLI (`gh`) or equivalent agent-native GitHub tools; use repository-scoped permissions appropriate to reading Issues/checks and writing authorized comments, labels and PRs.
- GitHub Issues and PRs enabled. All implementation tasks link to an Issue, even if they start as a chat request.
- Existing project verification tools; no additional test framework required by this template.
- Independent agent review capability (separate agent or reviewer session) before PASS. Without it, hand off without PASS rather than substitute self-review.

Local installation can finish without remote access, but remote label creation and live queue/CI checks remain explicitly pending. Never interpret access errors as an empty queue or green checks.

## Day-to-day prompts

| Intent | Examples | Entry point |
| --- | --- | --- |
| Discover candidates | `다음 작업 진행해`, `할 수 있는 작업 보여줘`, `이어서 진행해`, `이 Repository 작업 시작해` / “show next tasks” | [select-next-task](.agents/skills/select-next-task/SKILL.md) |
| Read-only status | `상태 요약해줘`, `현재 작업 큐 보여줘`, `진행 상황 알려줘` / “summarize status” | [workflow-status](.agents/skills/workflow-status/SKILL.md) |
| Mature an Issue | `이슈 #12 분석해줘`, `요구사항 정리해줘`, `큰 작업 쪼개줘` | [triage-and-spec](.agents/skills/triage-and-spec/SKILL.md) |
| Implement selected Issue | `이슈 #12 구현해줘`, `작업 진행해`, `기능 개발/버그 수정` | [implement-issue](.agents/skills/implement-issue/SKILL.md) |
| Improve selected PR | `PR #34 리뷰해줘`, `PR 피드백 반영해줘`, `검증 및 PASS 판정` | [review-and-pass-pr](.agents/skills/review-and-pass-pr/SKILL.md) |
| Initialize | `워크플로 초기화해줘` | [init-workflow](.agents/skills/init-workflow/SKILL.md) |

Discovery presents candidates and waits for your selection. A specific Issue/PR request is already a selection, but cannot bypass claims or dependencies. Completing one item never starts the next automatically.

## Architecture

| Path | Role |
| --- | --- |
| [AGENTS.md](AGENTS.md) | Minimal intent routing and skill/path index, not a workflow manual |
| [CLAUDE.md](CLAUDE.md) | Claude context import; preserve existing content when installing |
| [.agents/config.md](.agents/config.md) | Repo-owned policy and detected verification commands |
| [.agents/reference/workflow-model.md](.agents/reference/workflow-model.md) | Canonical queue, snapshot, dependency, state and PASS contracts |
| [.agents/reference/compatibility.md](.agents/reference/compatibility.md) | Agent discovery, tool equivalence and smoke checklist |
| [.agents/skills/](.agents/skills/) | Six Task Skills and five reusable Capability Skills |
| [.agents/tasks/](.agents/tasks/) | One free-form `<issue-number>.md` task/handoff file per Issue |
| `docs/` when present | Project/domain/architecture knowledge; never operational task state |

Skills are physically flat: `.agents/skills/<skill-name>/SKILL.md`. The `layer: task` or `layer: capability` frontmatter distinguishes layers; nested `task/<name>` or `capability/<name>` directories would defeat one-level discovery. Every skill includes RFC 2119 contracts and a Self-Check.

Task Skills express goals: status, candidate selection, issue triage/spec, implementation, PR improvement/PASS, initialization. Capability Skills provide shared procedures: [GitHub state](.agents/skills/github-state-and-labels/SKILL.md), [spec management](.agents/skills/spec-management/SKILL.md), [Git/worktrees](.agents/skills/git-and-worktree/SKILL.md), [TDD/verification](.agents/skills/tdd-and-verification/SKILL.md), and [task/handoff](.agents/skills/task-file-and-handoff/SKILL.md). Links, not duplicated manuals, connect them.

## Workflow lifecycle

### Progressive spec and dependency DAG

Keep the original Issue body intact. Analyze and clarify in comments; consolidate when useful in `## Canonical Spec Snapshot` with Goal, Requirements, Scope & Out of Scope, Constraints, checkable Acceptance Criteria, Open Questions, and Dependencies. Read the latest snapshot **plus subsequent Issue/PR comments** before work. Clear existing specs do not require ceremonial rewriting.

Resolve minor ambiguity through documented assumptions. Ask before material scope/core behavior changes, deleting requirements or changing user intent. Never weaken criteria to fit broken code. For an oversized Issue, propose independently mergeable child Issues and a `Depends on #N` / `Blocked by #N` acyclic graph, obtain approval, then create children. All branch from main/default branch; stacked PRs are not the default. Every prerequisite must be CLOSED; unknown state is not satisfied.

### Queue and labels

The queue is **Incomplete PRs > Actionable Issues**, then **configured priority > FIFO** within each category. Unlabeled work follows configured tiers; multiple priority labels use the highest. Skip claimed, blocked, passed, dependency-unmet and draft PR work; skip Issues with any open linked PR. Check a PR's linked Issue too. Present PR and Issue candidates separately and wait for selection.

| Fixed state label | Meaning and transition |
| --- | --- |
| `agent:working` | Active claim; live recheck before claim, owner/session comment, post-claim race check; release on completion/handoff/blocking |
| `agent:passed` | PR has objective success **and** independent agent quality approval at final head; awaits human merge |
| `agent:blocked` | Human/external action required, documented in task file and GitHub comment |

Configurable default priorities: `priority:high`, `priority:medium`, `priority:low`. A label is not an atomic lock. Never reclaim another session's stale claim without user approval. New comments do not remove PASS: a human explicitly removes `agent:passed` to request rework, returning the PR to the incomplete-PR category if otherwise eligible.

### Implementation, PASS and handoff

Use Red → Green → Refactor, existing project verification, changed-path smoke checks, safe worktree handling and atomic commits on one logical PR branch. Worktrees are optional and may be user/IDE/agent-managed. Do not overwrite unrelated changes or require the same branch checked out twice.

Repair at most `K` iterations (default 3) across a session's verification and review, not 3 per skill. Then record root cause/attempts, release working claims, hand off, and stop. Tests alone cannot earn PASS: all Acceptance Criteria, applicable test/lint/typecheck/build and required CI checks must pass **and a distinct independent agent must approve quality/security/regression review** for the same final pushed head. Self-review is required but not independent. Pending or unavailable required evidence means unpassed handoff, not success.

Task files are secondary context: original intent → current spec plus later comments → code/Git → objective checks/CI → previous task file. Implementation plans are disposable. Before stopping, update task/handoff, apply configured retention, synchronize reachable Git state, post evidence, release claims and report. `clean-before-pass` promotes durable context then removes the temporary file before final checks; `gitignored-local` mirrors handoff in GitHub. Humans merge using configured squash/rebase/merge strategy. STOP after completion, handoff or blocking.

## Cross-agent compatibility

Codex and OMP load `AGENTS.md` and supported repo-local `.agents/skills/<name>/SKILL.md` discovery. Claude Code loads `CLAUDE.md` importing `@AGENTS.md` and follows skill links; native skill-command discovery may require an approved optional `.claude/skills` bridge. No global installation is necessary. Provider/version behavior must be checked in the actual target environment; file-layout compatibility is not a claim of having executed every agent. See the [compatibility checklist](.agents/reference/compatibility.md).

## Template verification

This repository is a Markdown instruction template, not an executable workflow engine. Validate it without installing a runtime dependency:

1. Confirm eleven skills sit exactly one level under the skills directory; parse their YAML frontmatter and verify matching names, nonempty descriptions, task/capability layer, contract language and final Self-Check.
2. Resolve every local Markdown link and the Claude import. Inline target examples such as numeric task paths, absent target manifests, optional agent bridges and `docs/` are not required template files.
3. Walk through these concrete scenarios using the linked contracts (fixtures are not live GitHub mutations):

| Scenario | Route and required observations |
| --- | --- |
| A: initialize an existing target | Prompt → index → init; detect stack/commands, preserve agent rules and CI/hooks, resolve all six policy categories, merge selectively, honor commit policy and stop |
| B: discover next work | Index → selection → GitHub/model; exclude all state/dependency/draft/linked conflicts, rank PR before Issue then priority/FIFO, separate categories, wait for selection |
| C: raw Issue to completion | Triage → spec → implementation → review; preserve body, resolve ambiguity, snapshot if needed, TDD and smoke, bound K, reconstruct truth-first handoff, require both PASS gates or release claim and hand off; stop |
| D: post-PASS rework | New comment alone leaves PASS intact; human removes label; PR becomes eligible ahead of Issues, selected reviewer reads newer Issue/PR feedback, repairs within K, refreshes final-head evidence and independent review, re-applies PASS and stops |

4. Inspect Git status on main: all template files tracked, no temporary validation scripts/checkouts. Actual target installation, GitHub permissions/labels/CI and each installed agent need their own environment-specific smoke run; structural checks cannot prove those integrations.
