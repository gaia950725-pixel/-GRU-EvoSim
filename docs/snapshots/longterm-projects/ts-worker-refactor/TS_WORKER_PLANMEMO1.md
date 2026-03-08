# TS_WORKER_PLANMEMO1 — TS/웹워커 선결 사항 및 설계 방향

**작성일:** 2026-03-05
**기준 버전:** v27.20_fix1
**위치:** `docs/snapshots/longterm-projects/ts-worker-refactor/TS_WORKER_PLANMEMO1.md`
**연관 문서:**
- `TS_WORKER_SNAPSHOT.md` — 현황조사
- `HARD_MODULE_BLUEPRINT.md` — 서브시스템 분류표

---

## 1. TS패치 착수 전 선결 사항 목록

### [PRE-01] `_hs3_layerPredators` agentGrid 직접 참조 해소
**심각도:** 높음 (TS 전 반드시 해결)

**현황:**
```javascript
function _hs3_layerPredators(ctx) {
    const agentGrid = ctx.agentGrid;  // ← 직접 참조
    EvoSim.query.forEachAgentCell(agentGrid, p.pos.x, p.pos.y, ...)
}
```

**문제:** SWEEP 단계에서 Snapshot.Pre를 우회하고 live agentGrid를 직접 읽음. 웹워커 도입 시 워커가 메인 스레드의 live grid에 접근하는 구조가 되어 동시성 문제 발생.

**해결 방향:** ctx에 `agentGrid` 대신 `snapshot.agentGridPre`를 전달하도록 변경. 또는 Snapshot.Pre 구성 시 agentGrid 스냅샷 포함.

**블록 주석 현황:** `SIM_COLLISION_PREDATOR`에 `// depends: agentGrid — needs fix` 기재 예정.

---

### [PRE-02] `_hs3_layerResource` foodGrid 직접 참조 정리
**심각도:** 중간

**현황:** `EvoSim.query.forEachFoodCell(foodGrid, ...)` — live foodGrid 직접 접근.

**문제:** PRE-01과 동일 패턴. foodGrid는 agentGrid보다 변경 빈도가 낮으나 SWEEP 원칙상 Snapshot.Pre 경유가 맞음.

**해결 방향:** Snapshot.Pre에 foodGrid 스냅샷 포함 여부 결정 필요. 또는 eat 레이어를 COMMIT으로 이관하는 설계 검토.

---

### [PRE-03] SWEEP/COMMIT 경계 전 블록 명시적 준수 확인
**심각도:** 중간

**현황:** TS_PATCH 26.10 정책은 문서화되어 있으나, 각 블록이 실제로 정책을 준수하는지 코드 레벨 검증이 없음.

**필요 작업:**
- `SIM_COLLISION_*` 각 블록에 TS 귀속 주석 추가 (SWEEP vs COMMIT)
- COMMIT 전용 처리(`_hs3_commitNewborns` 등)가 SWEEP 내에 혼재하는 패턴 전수 확인
- `TS_pendAgents` 호출 지점 전수 확인 — SWEEP 내 큐 적재만 있는지 검증

---

### [PRE-04] 틱 파이프라인 실제 순서 문서화
**심각도:** 낮음 (설계 명확화)

**현황:** 사용자 제시 순서(이동/인식-생각/충돌/개인행동/출생사망/음향)와 코드 내 실제 실행 순서가 다를 수 있음. `// ORDER LOCKED` 주석이 collision 내부에만 있고, 틱 전체 파이프라인 문서가 없음.

**필요 작업:** `HS_FITTER_P4_1_SIM_LOOP`(→ `SIM_LOOP_CORE`) 실제 코드를 읽어 단계별 실행 순서 확정 후 `ARCHITECTURE_OVERVIEW` 또는 별도 문서에 기재.

---

### [PRE-05] 웹워커 인터페이스 계약 설계
**심각도:** 높음 (워커 구현 전 확정 필요)

**확정된 워커 범위:** 인식 + 생각 + 충돌 전체

**미결 설계 항목:**

| 항목 | 현황 | 결정 필요 내용 |
|------|------|-------------|
| 워커 입력 포맷 | 미정 | Snapshot.Pre 직렬화 구조 (transferable vs structured clone) |
| 워커 출력 포맷 | 미정 | 델타 배열 스키마 (agent별 position delta, health delta 등) |
| 이벤트 큐 구조 | 미정 | combat/share/repro/birth 이벤트 타입 정의 |
| 워커 수 | 미정 | 단일 워커 vs 레이어별 다중 워커 |
| SharedArrayBuffer 사용 여부 | 미정 | Atomics 필요 여부 |

---

## 2. TS패치 작업 순서 (권고)

```
Step 1: PRE-04 — 틱 파이프라인 실제 순서 문서화 (코드현황조사)
Step 2: PRE-01 — agentGrid 직참조 해소 (Snapshot.Pre 경유)
Step 3: PRE-02 — foodGrid 직참조 정리 또는 설계 방향 확정
Step 4: PRE-03 — SWEEP/COMMIT 경계 전 블록 준수 검증 + 주석 추가
Step 5: PRE-05 — 웹워커 인터페이스 계약 설계 (정책확정 필요)
Step 6: 웹워커 구현
Step 7: 하드모듈화 파일 분리 실행 (HARD_MODULE_BLUEPRINT.md 기준)
```

---

## 3. 하드모듈화 파일 분리 실행 조건 (트리거)

TS패치 완료로 간주하는 기준:

- [ ] PRE-01 해소 (agentGrid Snapshot.Pre 경유)
- [ ] PRE-02 방향 확정 및 적용
- [ ] PRE-03 전 블록 SWEEP/COMMIT 주석 완비
- [ ] PRE-05 웹워커 인터페이스 계약 확정
- [ ] 웹워커 통합 테스트 통과

위 조건 충족 후 `HARD_MODULE_BLUEPRINT.md` M01~M13 기준으로 파일 분리 실행.

---

## 4. 미결 정책 사항

| ID | 항목 | 현황 |
|----|------|------|
| PQ-TS-01 | `_hs3_layerResource`(eat)를 SWEEP 유지 vs COMMIT 이관 | 미결 |
| PQ-TS-02 | 웹워커 수 — 단일 워커 vs 레이어별 다중 워커 | 미결 |
| PQ-TS-03 | SharedArrayBuffer 사용 여부 | 미결 |
| PQ-TS-04 | 틱 파이프라인 공식 순서 문서 위치 (`ARCHITECTURE_OVERVIEW` vs 별도) | 미결 |
