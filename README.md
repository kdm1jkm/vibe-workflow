# Repository-native Agentic Development Workflow

Codex, Claude Code, OMP(oh-my-pi)에서 쓰는 재사용 가능한 초기화 템플릿입니다. 대상 저장소에 GitHub 중심의 repo-local 워크플로를 설치합니다: Issue → 점진적 Spec → TDD 구현 → PR → 자체 리뷰 → 독립 리뷰 → PASS → 사람의 merge.

워크플로는 Markdown 계약과 표준 Git/GitHub 인터페이스로만 구성됩니다. 별도 오케스트레이터, 데몬, 에이전트 전용 어댑터, 전역 설정, 템플릿과의 지속 동기화가 필요 없습니다. 초기화 후에는 대상 저장소가 정책과 스킬을 소유합니다.

기본 언어는 한국어입니다. 에이전트는 사용자 응답, Issue/PR 코멘트, Spec 내용, PR 본문, 작업 파일을 한국어로 씁니다. 스킬 계약 본문과 워크플로 제목(`## Canonical Spec Snapshot`, `### Acceptance Criteria` 등), 라벨, 경로, 명령은 영어 그대로 둡니다. 에이전트가 이 제목으로 산출물을 찾기 때문입니다. 언어는 초기화할 때 바꿀 수 있습니다([언어 정책](.agents/config.md)).

## 빠른 시작

**대상 저장소에서** 코딩 에이전트를 열고 아래 프롬프트를 그대로 붙여넣으세요:

```text
https://github.com/kdm1jkm/vibe-workflow.git 의 .agents/skills/init-workflow/SKILL.md를 읽고 그대로 따라서 이 저장소에 워크플로를 초기화해줘. 템플릿은 이 저장소 밖 임시 위치에서 읽어.
```

나머지는 [init-workflow](.agents/skills/init-workflow/SKILL.md)가 처리합니다. 대상 저장소 분석(스택, 검증 명령, 기존 에이전트 설정, CI/hooks, 라벨, 언어), 미정 정책 7개(priority 라벨, merge 전략, MCP, 작업 파일 보존, 충돌 해결, 커밋 정책, 언어)를 한 번에 묻기, 선택적 병합 설치, 검증, 보고 후 멈춤까지입니다.

비어 있는 **새** 저장소라면 이 저장소를 GitHub template으로 써서 만든 다음, 그 저장소에서 초기화 스킬을 실행해 실제 정책과 스택을 감지하게 해도 됩니다. 함께 들어 있는 config 기본값을 초기화가 끝난 상태로 보면 안 됩니다.

### 필요 조건과 접근 권한

- Git, 인증된 GitHub CLI(`gh`) 또는 그에 준하는 에이전트 내장 GitHub 도구. Issue/체크 읽기, 승인된 코멘트·라벨·PR 쓰기에 맞는 저장소 범위 권한.
- GitHub Issues와 PR 활성화. 채팅 요청으로 시작했더라도 모든 구현 작업은 Issue에 연결됩니다.
- 프로젝트의 기존 검증 도구. 이 템플릿이 새 테스트 프레임워크를 요구하지 않습니다.
- PASS 전 독립 에이전트 리뷰(별도 에이전트나 리뷰어 세션). 없으면 자체 리뷰로 대신하지 않고 PASS 없이 handoff합니다.

원격 접근이 없어도 로컬 설치는 끝낼 수 있지만, 원격 라벨 생성과 실시간 큐/CI 확인은 미완료로 명시됩니다. 접근 오류를 빈 큐나 통과한 체크로 해석하지 않습니다.

## 일상 명령

| 의도 | 예시 | 진입점 |
| --- | --- | --- |
| 후보 찾기 | `다음 작업 진행해`, `할 수 있는 작업 보여줘`, `이어서 진행해`, `이 Repository 작업 시작해` | [select-next-task](.agents/skills/select-next-task/SKILL.md) |
| 읽기 전용 상태 | `상태 요약해줘`, `현재 작업 큐 보여줘`, `진행 상황 알려줘` | [workflow-status](.agents/skills/workflow-status/SKILL.md) |
| Issue 구체화 | `이슈 #12 분석해줘`, `요구사항 정리해줘`, `큰 작업 쪼개줘` | [triage-and-spec](.agents/skills/triage-and-spec/SKILL.md) |
| 선택한 Issue 구현 | `이슈 #12 구현해줘`, `작업 진행해`, `기능 개발/버그 수정` | [implement-issue](.agents/skills/implement-issue/SKILL.md) |
| 선택한 PR 개선 | `PR #34 리뷰해줘`, `PR 피드백 반영해줘`, `검증 및 PASS 판정` | [review-and-pass-pr](.agents/skills/review-and-pass-pr/SKILL.md) |
| 초기화 | `워크플로 초기화해줘` | [init-workflow](.agents/skills/init-workflow/SKILL.md) |

후보 찾기는 후보를 보여주고 사용자의 선택을 기다립니다. 특정 Issue/PR을 지정하면 그것이 곧 선택이지만, claim이나 의존성 검사를 건너뛰지는 않습니다. 작업 하나가 끝나도 다음 작업을 자동으로 시작하지 않습니다.

## 구조

| 경로 | 역할 |
| --- | --- |
| [AGENTS.md](AGENTS.md) | 의도 라우팅과 스킬/경로 최소 인덱스. 워크플로 매뉴얼이 아님 |
| [CLAUDE.md](CLAUDE.md) | Claude용 context import. 설치 시 기존 내용 보존 |
| [.agents/config.md](.agents/config.md) | 저장소가 소유하는 정책(언어 포함)과 감지된 검증 명령 |
| [.agents/reference/workflow-model.md](.agents/reference/workflow-model.md) | 큐, 스냅샷, 의존성, 상태, PASS의 기준 계약 |
| [.agents/reference/compatibility.md](.agents/reference/compatibility.md) | 에이전트별 검색 방식, 도구 동등성, 스모크 체크리스트 |
| [.agents/skills/](.agents/skills/) | Task Skill 6개와 재사용 Capability Skill 5개 |
| [.agents/tasks/](.agents/tasks/) | Issue마다 하나씩 쓰는 자유 형식 `<issue-number>.md` 작업/handoff 파일 |
| `docs/` (있는 경우) | 프로젝트/도메인/아키텍처 지식. 에이전트 작업 상태는 두지 않음 |

스킬은 물리적으로 평평합니다: `.agents/skills/<skill-name>/SKILL.md`. 계층 구분은 frontmatter의 `layer: task` / `layer: capability`로 합니다. `task/<name>`, `capability/<name>`처럼 중첩하면 한 단계만 찾는 스캐너가 스킬을 놓칩니다. 모든 스킬에는 RFC 2119 계약과 Self-Check가 있습니다.

Task Skill은 목표 단위입니다: 상태, 후보 선택, Issue 분석/Spec, 구현, PR 개선/PASS, 초기화. Capability Skill은 공용 절차입니다: [GitHub 상태](.agents/skills/github-state-and-labels/SKILL.md), [Spec 관리](.agents/skills/spec-management/SKILL.md), [Git/worktree](.agents/skills/git-and-worktree/SKILL.md), [TDD/검증](.agents/skills/tdd-and-verification/SKILL.md), [작업 파일/handoff](.agents/skills/task-file-and-handoff/SKILL.md). 매뉴얼을 복제하지 않고 링크로 연결합니다.

## 워크플로 흐름

### 점진적 Spec과 의존성 DAG

원본 Issue 본문은 그대로 둡니다. 분석과 질문은 코멘트로 하고, 정리가 필요할 때 `## Canonical Spec Snapshot` 코멘트로 통합합니다. 구성은 Goal, Requirements, Scope & Out of Scope, Constraints, 확인 가능한 Acceptance Criteria, Open Questions, Dependencies입니다. 작업 전에는 최신 스냅샷**과 그 이후의 Issue/PR 코멘트**를 모두 읽습니다. 이미 명확한 Spec은 형식적으로 다시 쓰지 않습니다.

사소한 모호함은 가정을 기록하고 진행합니다. 범위나 핵심 동작을 바꾸거나, 요구사항을 지우거나, 사용자 의도를 바꿀 때는 먼저 묻습니다. 깨진 코드에 맞추려고 기준을 낮추지 않습니다. Issue가 너무 크면 독립적으로 merge 가능한 하위 Issue와 `Depends on #N` / `Blocked by #N` 비순환 그래프를 제안하고, 승인받은 뒤 만듭니다. 모두 main/기본 브랜치에서 갈라지며, stacked PR은 기본이 아닙니다. 선행 Issue는 모두 CLOSED여야 하고, 상태를 모르면 충족되지 않은 것으로 봅니다.

### 큐와 라벨

큐 순서는 **미완료 PR > 착수 가능한 Issue**, 각 범주 안에서는 **설정된 priority > FIFO**입니다. 라벨 없는 항목은 설정된 단계 뒤에 오고, priority 라벨이 여러 개면 가장 높은 것을 씁니다. claim/blocked/passed 상태, 의존성 미충족, draft PR은 건너뛰고, 열린 PR이 연결된 Issue도 건너뜁니다. PR은 연결된 Issue의 상태도 확인합니다. PR 후보와 Issue 후보를 나눠 보여주고 선택을 기다립니다.

| 고정 상태 라벨 | 의미와 전이 |
| --- | --- |
| `agent:working` | 진행 중 claim. claim 직전 실시간 재확인, 소유 세션 코멘트, claim 후 경합 확인. 완료/handoff/blocked 시 해제 |
| `agent:passed` | 최종 head에서 객관적 검증 **과** 독립 에이전트 품질 승인을 모두 통과한 PR. 사람의 merge 대기 |
| `agent:blocked` | 사람/외부 조치 필요. 작업 파일과 GitHub 코멘트에 기록 |

priority 기본값은 `priority:high`, `priority:medium`, `priority:low`이며 바꿀 수 있습니다. 라벨은 원자적 잠금이 아닙니다. 다른 세션의 오래된 claim은 사용자 승인 없이 가져오지 않습니다. 새 코멘트가 달려도 PASS는 유지됩니다. 재작업을 원하면 사람이 `agent:passed`를 직접 제거하고, 그러면 다른 조건이 맞는 한 PR이 미완료 PR 범주로 돌아갑니다.

### 구현, PASS, handoff

Red → Green → Refactor, 프로젝트의 기존 검증, 변경 경로 스모크 확인, 안전한 worktree 처리, 하나의 논리적 PR 브랜치 위의 원자적 커밋을 따릅니다. worktree는 선택이며 사용자/IDE/에이전트 누가 만들어도 됩니다. 관련 없는 변경을 덮어쓰거나 같은 브랜치를 두 곳에서 체크아웃할 필요가 없습니다.

자체 수리는 세션 전체에서 최대 `K`번(기본 3)입니다. 스킬마다 3번이 아닙니다. 다 쓰면 원인과 시도를 기록하고 claim을 해제하고 handoff한 뒤 멈춥니다. 테스트 통과만으로는 PASS가 아닙니다. 모든 Acceptance Criteria, 해당 test/lint/typecheck/build, 필수 CI가 통과해야 하고, **별도 독립 에이전트가 같은 최종 push head에 대해 품질·보안·회귀 리뷰를 승인**해야 합니다. 자체 리뷰는 필수지만 독립 리뷰가 아닙니다. 필수 근거가 대기 중이거나 없으면 성공이 아니라 PASS 없는 handoff입니다.

작업 파일은 보조 맥락입니다. 우선순위: 원래 사용자 의도 → 현재 Spec과 이후 코멘트 → 코드/Git → 객관적 검사/CI → 이전 작업 파일. 구현 계획은 버려도 됩니다. 멈추기 전에 작업 파일/handoff 갱신, 보존 정책 적용, 가능한 Git 동기화, 근거 게시, claim 해제, 보고를 합니다. `clean-before-pass`는 오래 남길 내용을 옮긴 뒤 최종 검사 전에 임시 파일을 지우고, `gitignored-local`은 handoff를 GitHub에 미러합니다. merge는 사람이 설정된 squash/rebase/merge 전략으로 합니다. 완료, handoff, blocked 모두 그 자리에서 멈춥니다.

## 에이전트 호환성

Codex와 OMP는 `AGENTS.md`를 읽고, 지원하는 경우 repo-local `.agents/skills/<name>/SKILL.md`를 찾습니다. Claude Code는 `@AGENTS.md`를 import하는 `CLAUDE.md`를 읽고 스킬 링크를 따라갑니다. Claude의 네이티브 스킬 명령 검색이 필요하면 승인 후 선택적으로 `.claude/skills` 브리지를 둘 수 있습니다. 전역 설치는 필요 없습니다. 프로바이더/버전별 동작은 실제 대상 환경에서 확인해야 합니다. 파일 배치가 호환된다는 것이 모든 에이전트를 실행해 봤다는 뜻은 아닙니다. [호환성 체크리스트](.agents/reference/compatibility.md)를 참고하세요.

## 템플릿 검증

이 저장소는 실행 엔진이 아니라 Markdown 지침 템플릿입니다. 런타임 의존성 없이 검증합니다:

1. 스킬 11개가 skills 디렉터리 바로 아래 한 단계에 있는지 확인합니다. YAML frontmatter를 파싱해 이름 일치, 비어 있지 않은 description, task/capability layer, 계약 키워드, 마지막 Self-Check를 확인합니다.
2. 모든 로컬 Markdown 링크와 Claude import가 해석되는지 확인합니다. 번호가 붙은 작업 파일 경로, 대상의 매니페스트, 선택적 에이전트 브리지, `docs/` 같은 예시는 템플릿에 있어야 하는 파일이 아닙니다.
3. 연결된 계약을 따라 아래 시나리오를 점검합니다(fixture이며 실제 GitHub 변경이 아님):

| 시나리오 | 경로와 확인할 점 |
| --- | --- |
| A: 기존 저장소 초기화 | 프롬프트 → init 스킬. 스택/명령과 언어 감지, 에이전트 규칙·CI·hooks 보존, 정책 7개 결정, 선택적 병합, 커밋 정책 준수, 멈춤 |
| B: 다음 작업 찾기 | 인덱스 → selection → GitHub/model. 상태/의존성/draft/연결 충돌 제외, PR 먼저 그다음 priority/FIFO, 범주 분리, 선택 대기 |
| C: 원본 Issue부터 완료까지 | Triage → Spec → 구현 → 리뷰. 본문 보존, 모호함 해소, 필요 시 스냅샷, TDD와 스모크, K 제한, 진실 우선 handoff 재구성, PASS 두 조건 모두 충족 또는 claim 해제 후 handoff, 멈춤 |
| D: PASS 후 재작업 | 새 코멘트만으로는 PASS 유지. 사람이 라벨 제거 → PR이 Issue보다 앞서 후보가 됨 → 선택된 리뷰어가 새 Issue/PR 피드백을 읽고 K 안에서 수정, 최종 head 근거와 독립 리뷰를 새로 확보, PASS 재부여 후 멈춤 |

4. main에서 Git 상태를 확인합니다. 모든 템플릿 파일이 추적되고, 임시 검증 스크립트나 checkout이 남지 않아야 합니다. 실제 대상 설치, GitHub 권한/라벨/CI, 설치된 각 에이전트는 환경별로 따로 스모크 실행이 필요합니다. 구조 검사로는 이런 연동을 증명할 수 없습니다.
