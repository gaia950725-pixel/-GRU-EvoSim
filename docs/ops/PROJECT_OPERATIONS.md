# EvoSim Project Operations (GitHub Workflow Canonical)

## 1) Scope
이 문서는 GitHub 기반 작업 운영 규칙의 단일 소스다. 정책/철학은 `docs/governance/MASTER_PROMPT.md`, 산출물 포맷은 `docs/spec/SPEC_OUTPUTS.md`를 따른다.

## 2) Branch Strategy
- 버전 증가 작업 통합 순서: `release/*` → 운영 브랜치(예: `operative-vibe-coding`) → `main`
- 독립 라인(예: 27.04)과 상호 머지는 금지한다. 필요 시 cherry-pick은 사용자 정책 승인 후 수행.
- 같은 버전 작업은 **하나의 브랜치에서 누적**한다.

### Branch naming
1. `release/<version>-<task>` (버전 증가 작업 필수, 예: `release/27.02-locale`)
2. `fix/<task>` (버전 증가 없는 결함 수정)
3. `docs/<task>` (운영/문서 전용)
4. `feature/<task>` (버전 증가 없는 기능 탐색/실험)

### 개발팀(executing) 브랜치 (Claude Code / Codex)
- 개발팀(executing)도 동일한 네이밍 컨벤션(`feature/*`, `fix/*`, `docs/*`, `release/*`)을 따른다. 버전 갱신 작업은 예외 없이 `release/*`를 사용한다.
- 실행 환경이 자동 배정하는 세션 브랜치(`claude/<slug>`, `codex/<slug>`, `work/*`)는 세션 전용 레이어다. 세션 브랜치명은 작업자 임의 규칙이 아니며, 저장소의 표준 운영 규칙(`release/*`, `fix/*`, `docs/*`, `feature/*`)과 별도로 공존할 수 있다.
- 버전 증가 작업은 최종 PR 기준 브랜치/제목/커밋에서 `release/<version>-<task>` 식별자가 명확히 보여야 한다.

### Ops-only / docs-only 변경 규칙
- 시뮬 버전 번프 없는 순수 운영 변경(CHANGELOG 채우기, 거버넌스 문서 수정, CLAUDE.md 규칙 추가 등)은 **개발팀(executing) 워킹 브랜치에 직접 커밋**한다.
- 별도 `docs/<task>` 브랜치 불필요.
- 코드 동작 변경이 없으므로 `EvoSim_latest.html` / archive 스냅샷 갱신도 불필요.

## 3) Version Unit Rule
- 하나의 작업 흐름(기획→명세→실행→검증→통합)을 하나의 버전 번호로 묶는다.
- 동일 버전 내 프롬프트가 여러 개인 경우, 브랜치는 유지하고 커밋만 누적한다.
- 최종 통합(merge) 시점에 해당 버전이 완료된 것으로 본다.

## 4) PR/Commit Granularity
- 권장: 프롬프트별 **커밋** + 버전 단위 **PR 1개**.
- 대규모 변경은 Phase 단위 PR 분할 허용.
- 예외: 개발팀(executing) 요청이 승인되었거나 사용자 지시가 있는 경우, 프롬프트 세트 **일괄 적용**을 허용한다.
- 커밋 메시지 예시:
  - `[27.02][P1/4] season core`
  - `[27.02][P2/4] locale debug`


## 4-1) Prompt-set Batch Execution Mode (일괄 패치 모드)
- 기본 원칙: 프롬프트를 쪼개 순차 적용하지 않고, **총 프롬프트 세트를 먼저 확정한 뒤 일괄 패치**할 수 있다.
- 입력 단위: 확정된 프롬프트 세트 1묶음(목표/범위/금지사항/FINAL 지침 포함).
- 실행 단위: 단일 적용 윈도우에서 일괄 반영 + 단일 실행보고 생성.
- 사전 체크(필수): 대상 파일 목록, 충돌 가능 지점, 롤백 기준 커밋 정의.
- 사후 체크(필수): `EvoSim_latest.html`/archive/CHANGELOG 동기화 및 Merge Gate 재검증.
- 커밋 원칙(기본): 패치 단위 독립 커밋 생성(1 Prompt = 1 Commit).
- 커밋 원칙(예외): 일괄 적용 승인 시 패치별 커밋 생성을 강제하지 않는다.
- 실패 처리: 일괄 적용 시작 전 기준점 커밋(rollback anchor)을 명시하고, 세트 단위 롤백 가능성을 보장한다.


## 4-2) Version Title Lock (설계 A: 버전 누락 방지)
- 패치 실행 전 `target_version`를 단일 소스로 확정한다(예: `27.09`).
- 완료 검증은 아래 3종 교차검증을 모두 통과해야 한다.
  1) 타이틀/헤더 표기 버전
  2) CHANGELOG 버전 헤더
  3) archive 파일명 버전
- 위 3종 중 1개라도 불일치하면 PR Ready 처리 금지(수정 후 재검증).

## 5) Artifact & File Routing

### Canonical outputs
- 최신 정본: `docs/releases/EvoSim_latest.html`
- 변경 이력(archive): `docs/releases/archive/EvoSim_<version>_<patch-or-branch>.html`
- CHANGELOG 단일 파일: `docs/releases/EvoSim_CHANGELOG.txt`

### Archive naming policy
- archive는 `latest`를 덮어쓰지 않고, 버전별 산출물을 보존하기 위한 공간이다.
- 파일명은 버전 + 패치명을 포함한다.
- 기본은 `버전+패치명` 스냅샷이며, 모든 커밋마다 n1/n2를 강제하지 않는다.
- `FINAL` / `fixN` 접미사는 릴리스급 마일스톤(최종 통합 직전 스냅샷, FINAL 이후 후속 수정)에서만 선택 적용한다.
- 예시:
  - `EvoSim_27.03_maritime_sot_FINAL.html`
  - `EvoSim_27.02_locale.html`

### Update mapping (must)
- 코드 동작 변경: `EvoSim_latest.html` + `archive` 스냅샷
- 중간/중요 패치: 위 + `EvoSim_CHANGELOG.txt`
- 경미 패치: CHANGELOG 항목 생략 가능(가독성 우선)
- 중요 패치: 코드 내 `PATCH RECORD` 또는 `ARCHITECTURE_OVERVIEW` 주석 갱신을 권장한다(구조변경이면 ARCHITECTURE_OVERVIEW 우선).
- 정책 변경: `docs/governance/MASTER_PROMPT.md`의 `last update` 갱신

## 6) Merge Gate (Definition of Done)
머지 전 체크리스트:
1. latest 동기화 완료(`docs/releases/EvoSim_latest.html`)
2. archive 스냅샷 생성 완료(버전+패치명 파일명)
3. changelog 갱신 완료(`docs/releases/EvoSim_CHANGELOG.txt`, 중간/중요 패치인 경우)
4. 실행보고가 `docs/spec/SPEC_OUTPUTS.md`의 고정 4섹션 규격을 충족함
5. 버전 증가 작업은 `release/*` 브랜치 규칙을 준수함
6. `target_version` 기준 3종 교차검증(타이틀/CHANGELOG/archive 파일명) 일치 확인

## 7) Responsibility Split

### RACI
| 의사결정/작업 | 사용자 | 기획팀(planning) | 개발팀(executing) |
| --- | --- | --- | --- |
| 정책 충돌 최종 판정 | **A** | C | C |
| 작업명세/프롬프트 품질 기준 수립 | A | **R** | C |
| 구현 적용 방식(코드/브랜치/커밋) | I | C | **R/A** |
| 실행보고/검증로그 작성 | I | C | **R** |
| PR 머지 승인 | **A/R** | C | I |

- 상충이 발생하면 개발팀(executing)/기획팀(planning)은 대안과 근거를 보고하고, 사용자가 최종 판정한다.
- 검수 루프의 종료 판정은 사용자에게 있으며, 운영상 종료 이벤트는 "프롬프트 재산출본 확정"으로 본다.

## 8) Relative-path Rule (for AI prompts)
- 프롬프트 입출력 경로는 저장소 상대경로만 사용한다.
- 절대경로(`/mnt/data` 등) 사용 금지.

### 상대경로 예시
- 저장소 루트가 기준일 때:
  - `docs/releases/EvoSim_latest.html` ✅ (상대경로)
  - `/mnt/data/EvoSim_latest.html` ❌ (절대경로)
