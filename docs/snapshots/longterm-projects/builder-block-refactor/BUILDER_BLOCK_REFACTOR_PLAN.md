# BUILDER_BLOCK 구획화 기획안

**작성일:** 2026-03-04
**기준 버전:** EvoSim 27.20_fix1
**최신 반영:** 27.20 Hard module base FINAL (해소 이슈 반영)
**연관 문서:**
- `BUILDER_BLOCK_SNAPSHOT.md` — 블록 전수조사 및 누락 구간 상세 분석
- `BUILDER_BLOCK_SNAPSHOT_AUDIT.md` — Codex 감사 기록

---

## 1. 기본 원칙

1. **비파괴 원칙**: 블록 마커는 주석만 삽입이며 코드 동작을 변경하지 않음.
2. **최소 단위**: 100줄 이상이면 반드시 최상위 블록으로 명명. 50줄 이상 논리 단위는 권장.
3. **계층 일관성**: 기존 네이밍 패턴(`SUBSYSTEM_ROLE` 또는 `HS_<phase>_<role>`) 준수.
4. **중복명 금지**: `_STEP1` / `_STEP2` suffix 또는 블록 분리로 해결.

---

## 2. GAP 처리 우선순위 및 실행 계획

우선순위 산정 기준: **규모 × 편집 충돌 위험 × 기능 중요도**

| 우선순위 | GAP | 라인 범위 | 규모 | 추천 블록명 | 단일/분할 | 작업 난이도 |
|---------|-----|---------|------|------------|---------|-----------|
| **P2** | GAP-F | 15186–15335 | ~150줄 | `UI_SYSTEM_PANELS_UPDATE` | 단일 | 낮 (함수 1개) |
| **P2** | GAP-D | 13600–13714 | ~115줄 | `FOOD_GRID_CORE` | 단일 | 낮 (클래스+유틸 묶음) |
| **P3** | GAP-E | 14727–14828 | ~102줄 | `BRAIN_CANVAS_DRAW` | 단일 | 낮 (함수 1개) |
| **P3** | GAP-G | 15790–15875 | ~86줄 | `FOOD_PHASE0_SIM_UTILS` | 단일 | 중 (Phase0 정책 코드 혼합) |
| **P4** | GAP-A | 8037–8093 | ~57줄 | `TS_SOCIAL_VOICE_GLOBALS` | 단일 | 낮 |
| **P4** | GAP-I | 18397–18453 | ~57줄 | `UI_CHIP_TABS_PNREV_INIT` | 단일 | 낮 |


## 3. 대형 기존 블록 내부 분할 권장

1,000줄 내외 미분할 대형 블록 4개. GAP 처리 완료 후 **별도 이슈**로 진행 권장.

| 블록명 | 현재 범위 | 현재 규모 | 분할 제안 |
|--------|---------|---------|---------|
| `UI_NAV_ROUTING` | 9717–11469 | ~1752줄 | `UI_NAV_WORLDSTART_INIT` + `UI_NAV_SIM_INIT` + `UI_NAV_RENDER_INIT` 3분할 |
| `HS_COLLISION_LOOP` | 13715–14726 | ~1011줄 | `COLLISION_SPATIAL_QUERY` + `COLLISION_APPLY_EAT` + `COLLISION_APPLY_FIGHT` 3분할 |
| `AGENT_GRID_CORE` | 8094–9079 | ~985줄 | `AGENT_CLASS_CORE` + `AGENT_NN_SENSOR` + `AGENT_GENETICS_CORE` 3분할 |
| `HS_PHASE1_HELPERS` | 16200–17093 | ~893줄 | `PHASE1_PREFRAME_HELPERS` + 기존 `HS_FITTER_P4_1_SIM_LOOP` 유지 (이미 서브블록 존재) |

---


## 4. 네이밍 컨벤션

기존 코드에서 확인된 패턴 3종:

| 패턴 | 예시 | 사용 맥락 |
|------|------|---------|
| `SUBSYSTEM_ROLE` | `FOOD_GRID_CORE`, `GAIA_SOT_CORE` | 신규 서브시스템 블록 **(신규 GAP 블록 권장)** |
| `HS_<phase>_<role>` | `HS_AGRO_HARVEST`, `HS_COLLISION_LOOP` | 핫픽스·이식 기원 블록 |
| `HS_FITTER_P<n>_<n2>_<role>` | `HS_FITTER_P4_1_INPUT_BINDINGS` | Fitter 패치 기원 블록 |

> **원칙**: `HS_` prefix는 핫픽스·이식 맥락에서만 사용. 신규 GAP 블록은 `SUBSYSTEM_ROLE` 패턴으로 통일.

---

## 5. 블록 추가 후 검증 체크리스트

```
[ ] 신규 START/END 마커 쌍이 대칭인지 확인
[ ] 기존 코드가 마커 주석 삽입 이외에 변경되지 않았는지 확인
[ ] 총 BUILDER_BLOCK 마커 수 재계산 (현재 236 → 추가 후 N)
[ ] 블록명 중복 없는지 grep -c "BUILDER_BLOCK: <name>" index.html 로 확인
[ ] BUILDER_BLOCK_SNAPSHOT.md 라인번호 재갱신
[ ] BUILDER_BLOCK_SNAPSHOT_AUDIT.md에 변경 이력 기재
```

---

## 6. 이슈 트래킹

| 이슈 ID | 유형 | 대상 | 위치 | 상태 |
|---------|------|------|------|------|
| ISS-02 | 대형 미분할 | `UI_NAV_ROUTING` | 9717–11469 (1752줄) | 중기 분할 권장 |
| ISS-03 | 대형 미분할 | `HS_COLLISION_LOOP` | 13715–14726 (1011줄) | 중기 분할 권장 |
| ISS-04 | 대형 미분할 | `AGENT_GRID_CORE` | 8094–9079 (985줄) | 중기 분할 권장 |
| ISS-05 | 대형 미분할 | `HS_PHASE1_HELPERS` | 16200–17093 (893줄) | 중기 분할 권장 |
| ISS-09 | GAP 누락 | GAP-F | 15186–15335 (150줄) | P2 |
| ISS-10 | GAP 누락 | GAP-D | 13600–13714 (115줄) | P2 |
| ISS-11 | GAP 누락 | GAP-E | 14727–14828 (102줄) | P3 |
| ISS-12 | GAP 누락 | GAP-G | 15790–15875 (86줄) | P3 |
| ISS-13 | GAP 누락 | GAP-A | 8037–8093 (57줄) | P4 |
| ISS-14 | GAP 누락 | GAP-I | 18397–18453 (57줄) | P4 |

---

*라인번호 유효 범위: v27.20_fix1 (18,534줄) 기준. 버전 변경 시 `BUILDER_BLOCK_SNAPSHOT.md` 재동기화 후 본 문서 이슈 라인번호도 갱신.*
