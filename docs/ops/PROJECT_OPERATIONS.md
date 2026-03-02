# EvoSim Project Operations (GitHub Workflow Canonical)

## 1) Scope
이 문서는 GitHub 기반 작업 운영 규칙의 단일 소스다. 정책/철학은 `docs/governance/MASTER_PROMPT.md`, 산출물 포맷은 `docs/spec/SPEC_OUTPUTS.md`를 따른다.

## 2) Branch Strategy
- 기본 통합 순서: `feature/*` → 운영 브랜치(예: `operative-vibe-coding`) → `main`
- 독립 라인(예: 27.04)과 상호 머지는 금지한다. 필요 시 cherry-pick은 사용자 정책 승인 후 수행.
- 같은 버전 작업은 **하나의 브랜치에서 누적**한다.

### Branch naming
- `release/<version>-<task>` (예: `release/27.02-locale`)
- `feature/<task>`
- `fix/<task>`
- `docs/<task>`

### AI 실행팀 브랜치 (Claude / Codex)
- AI 실행팀도 동일한 네이밍 컨벤션(`feature/*`, `fix/*`, `docs/*`, `release/*`)을 따른다.
- 실행 환경이 자동 배정하는 세션 브랜치(`claude/<slug>`, `codex/<slug>`)는 세션 전용이므로 **이 문서에 특정 브랜치명을 기재하지 않는다.** 현재 작업 브랜치는 시스템 프롬프트를 확인한다.

### Ops-only / docs-only 변경 규칙
- 시뮬 버전 번프 없는 순수 운영 변경(CHANGELOG 채우기, 거버넌스 문서 수정, CLAUDE.md 규칙 추가 등)은 **AI 워킹 브랜치에 직접 커밋**한다.
- 별도 `docs/<task>` 브랜치 불필요.
- 코드 동작 변경이 없으므로 `EvoSim_latest.html` / archive 스냅샷 갱신도 불필요.

## 3) Version Unit Rule
- 하나의 작업 흐름(기획→명세→실행→검증→통합)을 하나의 버전 번호로 묶는다.
- 동일 버전 내 프롬프트가 여러 개인 경우, 브랜치는 유지하고 커밋만 누적한다.
- 최종 통합(merge) 시점에 해당 버전이 완료된 것으로 본다.

## 4) PR/Commit Granularity
- 권장: 프롬프트별 **커밋** + 버전 단위 **PR 1개**.
- 대규모 변경은 Phase 단위 PR 분할 허용.
- 커밋 메시지 예시:
  - `[27.02][P1/4] season core`
  - `[27.02][P2/4] locale debug`

## 5) Artifact & File Routing

### Canonical outputs
- 최신 정본: `docs/releases/EvoSim_latest.html`
- 변경 이력(archive): `docs/releases/archive/EvoSim_<version>_<patch-or-branch>.html`
- CHANGELOG 단일 파일: `docs/releases/EvoSim_CHANGELOG.txt`

### Archive naming policy
- archive는 `latest`를 덮어쓰지 않고, 버전별 산출물을 보존하기 위한 공간이다.
- 파일명은 버전 + 패치명을 포함한다.
- 예시:
  - `EvoSim_27.03_maritime_sot_FINAL.html`
  - `EvoSim_27.02_locale.html`

### Update mapping (must)
- 코드 동작 변경: `EvoSim_latest.html` + `archive` 스냅샷
- 중간/중요 패치: 위 + `EvoSim_CHANGELOG.txt`
- 정책 변경: `docs/governance/MASTER_PROMPT.md`의 `last update` 갱신

## 6) Merge Gate (Definition of Done)
머지 전 체크리스트:
1. latest 동기화 완료
2. archive 스냅샷 생성 완료(버전+패치명 파일명)
3. changelog 갱신 완료(중간/중요)
4. 사용자 검정(플레이/콘솔) 완료
5. 독립 라인 정책 위반 없음(27.04 등)
6. 실행보고가 `docs/spec/SPEC_OUTPUTS.md` 실행보고 확장양식을 충족함

## 7) Responsibility Split
- 사용자: 정책 확정, 최종 머지 승인, 결과 검정
- 개발 실행팀(Codex/Claude): 브랜치/커밋/PR 생성 및 실행
- 기획/검토팀(ChatGPT/Gemini): 작업명세·검토·품질 리스크 제기

## 8) Relative-path Rule (for AI prompts)
- 프롬프트 입출력 경로는 저장소 상대경로만 사용한다.
- 절대경로(`/mnt/data` 등) 사용 금지.

### 상대경로 예시
- 저장소 루트가 기준일 때:
  - `docs/releases/EvoSim_latest.html` ✅ (상대경로)
  - `/mnt/data/EvoSim_latest.html` ❌ (절대경로)
