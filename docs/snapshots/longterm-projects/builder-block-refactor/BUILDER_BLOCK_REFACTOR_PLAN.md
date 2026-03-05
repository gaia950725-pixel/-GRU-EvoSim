# BUILDER_BLOCK 구획화 기획안

**작성일:** 2026-03-04  
**라인 재동기화:** 2026-03-05 (`v27.20_fix1` 재스캔 반영)  
**기준 버전:** EvoSim `v27.20_fix1` (18,534줄)

**연관 문서:**
- `BUILDER_BLOCK_SNAPSHOT.md` — 최신 라인번호 스냅샷
- `BUILDER_BLOCK_SNAPSHOT_AUDIT.md` — 자동 점검 로그

---

## 1. 기본 원칙

1. **비파괴 원칙**: 마커 주석 삽입 외 동작 코드 변경 금지.
2. **최소 단위**: 100줄 이상은 최상위 블록 권장, 50줄 이상은 서브블록 권장.
3. **계층 일관성**: 기존 네이밍 패턴(`SUBSYSTEM_ROLE`, `HS_<phase>_<role>`) 유지.
4. **중복명 금지**: suffix(`_STEP1`, `_LEGACY`, `_PHASE0`)로 명확히 분리.

---

## 2. 미해소 GAP 우선순위 (v27.20_fix1)

| 우선순위 | GAP | 라인 범위 | 규모 | 추천 블록명 | 작업 난이도 |
|---|---|---:|---:|---|---|
| P2 | GAP-F | 15200–15349 | ~150줄 | `UI_SYSTEM_PANELS_UPDATE` | 낮음 |
| P2 | GAP-D | 13614–13728 | ~115줄 | `FOOD_GRID_CORE` | 낮음 |
| P3 | GAP-E | 14741–14842 | ~102줄 | `BRAIN_CANVAS_DRAW` | 낮음 |
| P3 | GAP-G | 15804–15889 | ~86줄 | `FOOD_PHASE0_SIM_UTILS` | 중간 |
| P4 | GAP-A | 8035–8091 | ~57줄 | `TS_SOCIAL_VOICE_GLOBALS` | 낮음 |
| P4 | GAP-I | 18413–18469 | ~57줄 | `UI_CHIP_TABS_PNREV_INIT` | 낮음 |

> 완료됨: GAP-B/C/H (27.20 계열 반영 완료)

---

## 3. 대형 블록 분할 후보 (Builder Block 리팩토링 본체)

| 블록명 | 현재 범위 | 규모 | 분할 제안 |
|---|---:|---:|---|
| `UI_NAV_ROUTING` | 9726–11471 | ~1745줄 | `UI_NAV_WORLDSTART_INIT` / `UI_NAV_SIM_INIT` / `UI_NAV_RENDER_INIT` |
| `HS_COLLISION_LOOP` | 13729–14740 | ~1011줄 | `COLLISION_SPATIAL_QUERY` / `COLLISION_APPLY_EAT` / `COLLISION_APPLY_FIGHT` |
| `AGENT_GRID_CORE` | 8092–9077 | ~985줄 | `AGENT_CLASS_CORE` / `AGENT_NN_SENSOR` / `AGENT_GENETICS_CORE` |
| `HS_PHASE1_HELPERS` | 16214–17107 | ~893줄 | `PHASE1_PREFRAME_HELPERS` + `HS_FITTER_P4_1_SIM_LOOP` 유지 |

---

## 4. 이슈 트래킹 (라인 재동기화 반영)

| 이슈 ID | 유형 | 대상 | 위치 | 상태 |
|---|---|---|---:|---|
| ISS-01 | 중복 블록명 | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` | 18097–18102 / 18255–18260 | **해결됨** (`...LEGACY_STEP1` 분리) |
| ISS-02 | 대형 미분할 | `UI_NAV_ROUTING` | 9726–11471 | 중기 분할 권장 |
| ISS-03 | 대형 미분할 | `HS_COLLISION_LOOP` | 13729–14740 | 중기 분할 권장 |
| ISS-04 | 대형 미분할 | `AGENT_GRID_CORE` | 8092–9077 | 중기 분할 권장 |
| ISS-05 | 대형 미분할 | `HS_PHASE1_HELPERS` | 16214–17107 | 중기 분할 권장 |
| ISS-06 | GAP 누락 | GAP-F | 15200–15349 | P2 |
| ISS-07 | GAP 누락 | GAP-D | 13614–13728 | P2 |
| ISS-08 | GAP 누락 | GAP-E | 14741–14842 | P3 |
| ISS-09 | GAP 누락 | GAP-G | 15804–15889 | P3 |
| ISS-10 | GAP 누락 | GAP-A | 8035–8091 | P4 |
| ISS-11 | GAP 누락 | GAP-I | 18413–18469 | P4 |

---

## 5. 변경 후 검증 체크리스트

- [ ] START/END 대칭 확인
- [ ] depthEnd=0, negDepth=false 확인
- [ ] 블록명 중복 여부 확인
- [ ] `BUILDER_BLOCK_SNAPSHOT.md` / `BUILDER_BLOCK_SNAPSHOT_AUDIT.md` 동시 갱신

