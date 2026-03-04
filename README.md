# choiji-guide

Claude Code 에이전트 및 스킬 설정을 공유하기 위한 템플릿 레포지토리입니다.

## 개요

소규모 개발팀이 Claude Code를 활용하여 PRD 작성부터 스프린트 계획, 구현, 마무리까지 일관된 워크플로우로 개발할 수 있도록 에이전트와 스킬을 정의합니다.

## 사용 방법

이 레포지토리의 `.claude/` 디렉토리를 여러분의 프로젝트에 복사하세요.

```bash
cp -r .claude/ /path/to/your-project/.claude/
```

복사 후, 각 에이전트 파일(`.claude/agents/*.md`)의 frontmatter에 절대 경로가 없는지 확인하세요. `memory: project` 설정이 런타임에 올바른 경로를 자동으로 주입합니다.

## 포함된 에이전트

| 에이전트 | 설명 |
|----------|------|
| `prd-to-roadmap` | `docs/PRD.md`를 분석하여 Agile 기반 `docs/ROADMAP.md`를 생성합니다. Playwright MCP 검증 시나리오를 각 Phase에 포함합니다. |
| `sprint-planner` | ROADMAP을 기반으로 스프린트 계획을 수립하고 `docs/sprint/sprint{N}.md`에 저장합니다. |
| `sprint-close` | 스프린트 완료 후 ROADMAP 상태 업데이트, PR 생성, 코드 리뷰, 자동 검증을 순서대로 처리합니다. |
| `code-reviewer` | 구현 완료된 코드를 계획 문서와 비교하여 Critical/Important/Suggestion 등급으로 이슈를 분류합니다. |

## 포함된 스킬

| 스킬 | 설명 |
|------|------|
| `writing-plans` | 기능 구현 전 단계별 실행 계획을 `docs/plans/YYYY-MM-DD-<feature>.md`에 작성합니다. TDD 기반으로 각 태스크를 2~5분 단위로 분해합니다. |
| `karpathy-guidelines` | 과도한 추상화, 불필요한 기능 추가, 외과적이지 않은 코드 변경을 방지하는 LLM 코딩 가이드라인입니다. |

## 스프린트 워크플로우

```
docs/PRD.md
    │
    ▼ prd-to-roadmap 에이전트
docs/ROADMAP.md
    │
    ▼ sprint-planner 에이전트
docs/sprint/sprint{N}.md
    │
    ▼ 구현 (writing-plans 스킬 → 코드 작성)
    │
    ▼ sprint-close 에이전트
    ├─ ROADMAP.md 상태 업데이트
    ├─ sprint{N} → main PR 생성
    ├─ code-reviewer subagent 코드 리뷰
    ├─ Playwright MCP 자동 검증
    └─ docs/sprint/sprint{N}/playwright-report.md 저장
```

## 전제 조건

- [Claude Code](https://claude.ai/code) 설치
- Playwright MCP 서버 설정 (UI 자동 검증 사용 시)
