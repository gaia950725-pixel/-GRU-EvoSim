# TS_WORKER_SNAPSHOT — EvoSim TS/웹워커 현황조사

**작성일:** 2026-03-05
**기준 버전:** v27.20_fix1
**위치:** `docs/snapshots/longterm-projects/ts-worker-refactor/TS_WORKER_SNAPSHOT.md`
**연관 문서:**
- `HARD_MODULE_BLUEPRINT.md` — 서브시스템 분류표, 파일 분리 조건
- `TS_WORKER_PLANMEMO1.md` — 선결 사항 및 설계 방향

---

## 1. TS(Tick Split) 정의 및 현재 정책

**TS = Tick Split.** 1틱 내 실행 순서 재배열 + 델타 누적 후 틱 말미 일괄 적용 패치.

현행 TS 정책 기준: `TS_PATCH 26.10` (JS_BOOTSTRAP_CORE 블록 내 문서화)

| 단계 | 역할 | 현행 규칙 |
|------|------|---------|
| **PRE** | 시간 진행 | decay/regen/TTL/catch-up, E2I rebuild, Snapshot.Pre 그리드 구성 |
| **SWEEP** | 판정 (읽기 전용) | Snapshot.Pre만 읽음. 직접 world/agent 쓰기 금지. 이벤트·델타 큐에 적재 |
| **COMMIT** | 일괄 적용 | 델타/이벤트/pending flush, health/dead/breath 확정, 다음 틱 캐시 무효화 |

**현행 정책 주요 항목:**
- Grids store IDs only (uid/hid/pid). 셀→엔티티 접근은 E2I(ID→idx)만 허용
- Spawn/birth/respawn/food-add → 다음 틱 반영 (SWEEP 큐, COMMIT flush)
- Collision 쿼리는 Snapshot.Pre 위치 기준 (post-move 아님)
- Shout 매 틱 평가, clamor grid 쓰기는 COMMIT만, decay는 PRE만
- DEAD 확정은 COMMIT에서만 (healthFinal ≤ 0 기준)

---

## 2. 현재 틱 파이프라인 (코드 기반 확인)

`HS_COLLISION_LOOP` 내 실행 순서 (`// ORDER LOCKED` 주석 존재):

```
_hs3_layerResource(ctx)          ← Agent vs Food eat (FOOD_SRC_NAT/FARM/HUNT_DROP)
for each agent:
  _hs3_layerHunt(ctx, a)         ← Agent vs Herbivore 사냥
  _hs3_layerAgentAgent(ctx, a)   ← Agent vs Agent (전투/share/번식)
_hs3_commitNewborns(ctx, ...)    ← 출생 TS_pendAgents 큐 적재 (SWEEP)
_hs3_layerPredators(ctx)         ← Predator vs Agent
```

**사용자 제시 틱 내 처리 단계 (실제 순서는 코드 재확인 필요):**
이동 / 인식-생각 / 충돌 / 개인행동 / 출생사망 / 음향

---

## 3. 웹워커 범위 확정

**웹워커 담당 범위 (확정):** 인식 + 생각 + 충돌 전체

| 워커 담당 | 해당 블록(안) |
|---------|------------|
| 인식 (시야·오클루전·음향 감지) | `VISION_OCCLUSION_GLOBALS`, `VISION_CACHE_DERIVED` |
| 생각 (NN 추론, 행동 결정) | `AGENT_GRID_CORE` 내 NN 파트 |
| 충돌 전체 | `SIM_COLLISION_CORE` 및 하위 6개 블록 |

**워커 경계 조건:**
- 워커 입력: Snapshot.Pre (positions, grids, 에이전트 상태 읽기 전용)
- 워커 출력: 델타 배열 + 이벤트 큐 → 메인 스레드 COMMIT에서 일괄 적용
- 워커 내부에서 직접 world/agent 쓰기 금지 (현행 SWEEP 원칙과 동일)

---

## 4. SIM_COLLISION 하위 블록 TS 귀속 현황

| 블록명 | 현행 위치 | TS 귀속 | 비고 |
|--------|---------|--------|------|
| `SIM_COLLISION_EAT` | SWEEP | SWEEP | foodGrid 직접 참조 — needs fix |
| `SIM_COLLISION_HUNT` | SWEEP | SWEEP | herbivoreGrid 참조 |
| `SIM_COLLISION_AGENT_FIGHT` | SWEEP | SWEEP | |
| `SIM_COLLISION_AGENT_PEACE` | SWEEP | SWEEP | share/repro 판정 |
| `SIM_COLLISION_BIRTH_PEND` | SWEEP | SWEEP(적재) / COMMIT(반영) | TS_pendAgents |
| `SIM_COLLISION_PREDATOR` | SWEEP | SWEEP | agentGrid 직접 참조 — needs fix |

---

## 5. 현재 코드에서 확인된 TS 위반 / 선결 필요 지점

| 위치 | 문제 | 심각도 |
|------|------|--------|
| `_hs3_layerPredators` | `agentGrid` 직접 참조 (Snapshot.Pre 우회) | 높음 |
| `_hs3_layerResource` | `foodGrid` 직접 참조 | 중간 |
| `SIM_COLLISION_BIRTH_PEND` | `TS_pendAgents` — 큐 적재는 SWEEP 적합하나 경계 명시 필요 | 낮음 |

> 상세 선결 사항 목록: `TS_WORKER_PLANMEMO1.md` 참조

---

## 6. 하드모듈화와의 연계 조건

- **파일 분리 실행 트리거:** TS패치 완료 후
- TS패치 완료 기준: agentGrid 직참조 해소 + SWEEP/COMMIT 경계 전 블록 준수 + 웹워커 인터페이스 확정
- 파일 분리 시 우선 분리 대상: `sim.js` 내 `SIM_COLLISION_*` 블록군 (워커 이관 단위와 일치)
