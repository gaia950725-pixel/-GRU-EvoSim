# HARD_MODULE_BLUEPRINT — EvoSim 하드모듈화 청사진

**작성일:** 2026-03-05 / **갱신일:** 2026-03-07
**기준 버전:** v27.21 (Phase 3-A 실행 후)
**위치:** `docs/snapshots/longterm-projects/builder-block-refactor/HARD_MODULE_BLUEPRINT.md`
**연관 문서:**
- `BUILDER_BLOCK_SNAPSHOT.md` — 블록 전수조사 및 라인번호
- `BUILDER_BLOCK_REFACTOR_PLAN.md` — GAP 처리 이슈 트래킹

---

## 1. 확정 정책 전체

| 항목 | 확정 내용 |
|------|---------|
| 모듈 단위 | 서브시스템 경계 (food/agent/gaia/ui 등) |
| 이번 작업 범위 | 비파괴(마커+명칭) + 이관 청사진 확정. 실행은 후속 프로젝트. |
| 파일 분리 실행 조건 | TS패치 완료 후 |
| Agent/Creature | `agent.js` + `creature.js` 분리 (NN 개체 vs 클래식 개체) |
| Bootstrap | `bootstrap.js` 독립 (레지스트리·이벤트버스는 진입점) |
| Config 혼재 정리 | `cfg_defaults.js`(순수 설정값) / `ui_config.js`(설정화면 UI) 분리 |
| Collision 파일 분리 | `sim.js` 내 블록 유지. 명칭은 지금 확정, 파일 분리는 TS패치 후 |
| HS_ rename | 가능. PATCH RECORD `- 27.21 (builder block refactor): renamed from HS_<원본>` 삽입 |
| DIRTY 접미사 | 임시 더티플래그 상태를 의미. rename 시 제거 가능 (기능 설명으로 대체) |
| LEGACY 접미사 | 의도적 레거시 표시. rename 시 코드 내 `// LEGACY: ...` 주석으로 대체 필요 |
| HTML 형식 블록 | `<!-- BUILDER_BLOCK: ... -->` 형식 블록은 Python 스크립트 자동화 불가 — 수동 Edit 처리 |
| TS 귀속 주석 | SWEEP/COMMIT 사실만 기재. 이관 목적지 미기재 (정책변경 충돌 방지) |
| 불일치 판정 기준 C1 | 블록명이 서브시스템 소속을 반영 못함 → Rename |
| 불일치 판정 기준 C2 | 블록이 2개 이상 서브시스템 코드 포함 → Split 또는 계층화 |
| 불일치 판정 기준 C3 | 블록 위치가 서브시스템 경계와 어긋남 → 이관 청사진 기록 (이번 미실행) |
| 불일치 판정 기준 C4 | HS_ prefix 유물, 상시 코드화됨 → 목적지 서브시스템 명으로 Rename |

---

## 2. 서브시스템 분류표 (M01~M13)

### M01 — `styles.css`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `STYLES` | 유지 | |
| `CSS` | 유지 | STYLES 내 |

### M02 — `bootstrap.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `JS_TOP` | 유지 | |
| `JS_BOOTSTRAP_CORE` | 유지 | 레지스트리·이벤트버스·기반구조 |

### M03 — `cfg_defaults.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `CFG_DEFAULTS` | 유지 | 순수 설정값 |
| `NN_IO_ENUMS` | 유지 | |

### M04 — `ui_config.js` (설정화면 UI)
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `WORLDGEN_UI_INIT` | `UI_CFG_SCREEN_INIT` | C2: config/worldgen 혼재 해소 |
| `CONFIG_TABS` | `UI_CFG_TABS` | UI_CFG_SCREEN_INIT 내 |
| `HS_FITTER_P1_3_GRAPH_FLAGS` | `UI_CFG_GRAPH_FLAGS` | C4 |
| `HS_FITTER_P1_3A_HISTORY_ADAPTER` | `UI_CFG_HISTORY_ADAPTER` | C4 |
| `HS_FITTER_P1_3_PANEL_EXPAND_DIRTY` | `UI_CFG_PANEL_EXPAND` | C4 |

### M05 — `gaia.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `GAIA_SOT_CORE` | 유지 | |
| `GAIA_MOUNTAINS` | 유지 | |
| `GAIA_MTN_POLICY` | 유지 | GAIA_MOUNTAINS 내 |
| `GAIA_ENV30` | 유지 | |
| `GAIA_30PX_LOOKUP_APIS` | 유지 | |
| `MAP_PRESETS` | 유지 | |
| `TIME_NARRATIVE_GLOBALS` | 유지 | |

### M06 — `agent.js` (NN 개체)
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `AGENT_GRID_CORE` | 유지 | ISS-04: 3분할 권장 유지 |
| `HS_18_7_GLOBALS` | `AGENT_GRID_GLOBALS` | C4, 내부 |
| `HS_18_7_BUILD_FUNC_REPLACE` | `AGENT_GRID_BUILD` | C4, 내부 |
| `GRID_QUERY_API` | 유지 | |
| `HS_QUERY_API` | `AGENT_QUERY_CORE` | C4, 내부 |
| `HS_18_7_ADD_AGENT_REPLACE` | `AGENT_GRID_ADD` | C4 |
| `GRIDDBG_HELPERS` | 유지 | |
| `VISION_CACHE_DERIVED` | 유지 | |
| `HS_OCCLUSION_21RAY_GLOBALS` | `VISION_OCCLUSION_GLOBALS` | C4 |

### M07 — `creature.js` (클래식 개체)
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `HERB_GRID_CORE` | 유지 | |
| `HS_A2_HERBGRID_GLOBALS` | `HERB_GRID_GLOBALS` | C4, 내부 |
| `HS_A2_BUILD_HERBGRID_REPLACE` | `HERB_GRID_BUILD` | C4, 내부 |
| `PRED_GRID_CORE` | 유지 | |
| `PREDATOR_CLASS_CORE` | 유지 | 27.20 신설 |
| `HERBIVORE_CLASS_CORE` | 유지 | 27.20 신설 |
| `SOLITARY_ACTION_HELPERS` | 유지 | 27.20 신설 |

### M08 — `food.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `AGRO_GLOBALS` | 유지 | |
| `AGRO_INIT_CORE` | 유지 | |
| `HS_INITAGRO_FARMBUILD_BLOCKS` | `AGRO_FARM_BUILD` | C4, 내부 |
| `HS_INITAGRO_FARMSUIT_BLOCKS` | `AGRO_FARM_SUIT` | C4, 내부 |
| `AGRO_FARM_WORK_TICK` | 유지 | |
| `HS_AGRO_WORK_REPIDX` | `AGRO_WORK_REPIDX` | C4, 내부 |
| `CORE_MATH_ENERGY_UTILS` | 유지 | |
| `HS_AGRO_BLOCKVIEW_UTILS` | `AGRO_BLOCKVIEW_UTILS` | C4 |
| `UI_INPUT_WORLD_UTILS` | C2 분할 → `UI_INPUT_COORD_UTILS` + `UI_AGRO_HOVER_INPUT` | M12 귀속 확정 (코드 직접 확인) |
| `HS_AGRO_TOUCH_REPIDX` | `AGRO_TOUCH_REPIDX` | C4 |
| `HS_AGRO_PEEK_TILE` | `AGRO_PEEK_TILE` | C4 |
| `HS_AGRO_TOP4_REPIDX` | `AGRO_TOP4_REPIDX` | C4 |
| `HS_AGRO_HARVEST` | `AGRO_HARVEST` | C4 |
| `HS_AGRO_HARVEST_REPIDX` | `AGRO_HARVEST_REPIDX` | C4, 내부 |
| `HS_AGRO_HARVEST_FARMDROP` | `AGRO_HARVEST_FARMDROP` | C4, 내부 |
| `FOOD_GRID_CORE` (GAP-D) | 유지 | 신설 예정 |

### M09 — `social.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `SURNAME_AUTO_CORE` | 유지 | |
| `SURNAME_AUTO_SANITIZE_CLAN` | 유지 | 내부 |
| `ERA_BTN_SURNAME_AUTO_WIRE` | 유지 | |
| `TS_SOCIAL_VOICE_GLOBALS` (GAP-A) | 유지 | 신설 예정 |
| `HS_19_CLAMOR_CORE_ADD` | `SOCIAL_CLAMOR_CORE` | C4 |

### M10 — `sim.js`

#### SIM_COLLISION 이중 하위 모듈 구조
```
SIM_COLLISION_CORE                     [HS-origin: HS_COLLISION_LOOP]
│  // depends: foodGrid, agentGrid, herbivoreGrid — TS 전 선결 필요
│
├── SIM_COLLISION_EAT                  [HS-origin: _hs3_layerResource]
│     Agent vs Food (NAT/FARM/HUNT_DROP)
│
├── SIM_COLLISION_HUNT                 [HS-origin: _hs3_layerHunt]
│     Agent vs Herbivore (kill + hunt drop)
│
├── SIM_COLLISION_AGENT_FIGHT          [HS-origin: _hs3_layerAgentAgent > wantsFight]
│     agentHitAgent (a↔b), warLog, invulnUntilTick
│
├── SIM_COLLISION_AGENT_PEACE          [HS-origin: _hs3_layerAgentAgent > !wantsFight]
│     share (donor/recv) + collisionReproduce
│
├── SIM_COLLISION_BIRTH_PEND           [HS-origin: _hs3_commitNewborns]
│     TS_pendAgents 큐 적재
│     // TS 귀속: SWEEP(적재), COMMIT(반영)
│
└── SIM_COLLISION_PREDATOR             [HS-origin: _hs3_layerPredators]
      // depends: agentGrid — needs fix (TS 전 선결)
      humanHitPred / predBiteHuman
```

#### 기타 sim.js 블록
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `SPAWN_AND_LOG_CORE` | 유지 | |
| `HS_FITTER_P1_1_RENDERLOGS_GUARD` | `SPAWN_RENDERLOGS_GUARD` | C4, 내부 |
| `HS_PHASE1_HELPERS` | `SIM_PHASE1_HELPERS` | C4, ISS-05 분할 권장 유지 |
| `HS_FITTER_P4_1_SIM_LOOP` | `SIM_LOOP_CORE` | C4, 내부 |
| `HS_19_SIMLOOP_DECAY_INSERT` | `SIM_LOOP_DECAY` | C4, 내부 |
| `HS_AGRO_SELECTED_TILE_LAZY_TOUCH` | 유지 | 내부 |
| `AUTOCLAN_ANIMATE_HOOK` | 유지 | 내부 |
| `HS_RAF_SINGLETON_21_1` | `SIM_RAF_SINGLETON` | C4 |
| `HS_DISPOSE_WORLD_21_1` | `SIM_DISPOSE_WORLD` | C4 |
| `HS_A3_DEV_PERF_API` | `SIM_DEV_PERF_API` | C4 |
| `HS_BRAIN_THROTTLE_HELPER` | `SIM_BRAIN_THROTTLE` | C4 |
| `HS_FITTER_P4_1_UITICK_REGION` | `UI_UITICK_REGION` | C4. 코드확인: DOM 패널 갱신 UI 클러스터 — SIM_ 접두사 오류 수정(27.22). → M12 귀속 |
| `HS_UITICK_WRAPPERS` | `UI_UITICK_WRAPPERS` | C4, 내부. 동일 수정 |
| `HS_FITTER_P1_4_UITICK500_ROUTER` | `UI_UITICK500_ROUTER` | C4, 내부. 동일 수정 |
| `SIM_GLOBALS_POST_GAIA_ENV30` | 유지 | M10 귀속 확정, C2 분할은 Phase 3-B |
| `ATLAS_P1_OVERVIEW_VARS` | → M11 이관 (C3) | 렌더 전용 변수 확인, render.js 귀속 확정 |
| `ATLAS_P1_STATICMAP_CLIP` | → M11 이관 (C3) | Canvas 2D 렌더 파이프라인 확인, render.js 귀속 확정 |
| `ATLAS_P1_OVERVIEW_CALL` | → M11 이관 (C3) | 내부 호출, render.js 귀속 확정 |
| `ATLAS_P1_OVERVIEW_FN` | → M11 이관 (C3) | buildOverviewCanvas() 렌더 함수, render.js 귀속 확정 |
| `HS_A3_POP_RING_REPLACE` | `POP_RING_CORE` | C4 ✓ 27.21 완료 |
| `HS_FITTER_P1_2_POP_SAMPLE` | `POP_SAMPLE_CORE` | C4 ✓ 27.21 완료 |
| `HS_APPLYEAT_FARM_DROP_STATS` | `SIM_APPLYEAT_FARMDROP_STAT` | C4, M10 확정(applyEat 내부 FARM 통계). Phase 3-B 처리 |
| `HS_NAT_AREA_SCALE` | `SIM_NAT_REGEN_SCALE` | C4, M10 확정(_hs3_layerResource 내 자연식량 재생). Phase 3-B 처리 |

### M11 — `render.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `HS_FITTER_P4_1_RENDER_LOOP` | `RENDER_LOOP_CORE` | C4 |
| `ATLAS_P1_OVERVIEW_VARS` | 유지 | C3 이관 확정 (현 M10 위치) |
| `ATLAS_P1_STATICMAP_CLIP` | 유지 | C3 이관 확정 (현 M10 위치) |
| `ATLAS_P1_OVERVIEW_CALL` | 유지 | C3 이관 확정, 내부 |
| `ATLAS_P1_OVERVIEW_FN` | 유지 | C3 이관 확정 (현 M10 위치) |
| `BRAIN_CANVAS_DRAW` (GAP-E) | 유지 | 신설 예정 |
| `CANVAS_CONTEXT_INIT` | 유지 | 27.20 신설 |
| `AGENT_DRAW_TAIL` | 유지 | 27.20 신설 |
| `HS_FITTER_P1_1_PATH2D_CACHE` | `RENDER_PATH2D_CACHE` | C4. 코드확인: Agent 렌더 Path2D 캐시(Canvas API). M12→**M11** 귀속 수정(27.22) |
| `HS_FITTER_P1_1_AGENT_TRI_REPLACE` | `AGENT_DRAW_TRI` | C4. 코드확인: Agent.prototype.draw() 내 삼각형 Canvas 렌더. M12→**M11** 귀속 수정(27.22) |

### M12 — `ui.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `UI_NAV_ROUTING` | 유지 | ISS-02: 3분할 권장 유지 |
| `HS_19_CLAMOR_RESET_ON_START_INSERT` | `UI_NAV_CLAMOR_RESET` | C4, 내부 |
| `HS_A3_POP_RESET_REPLACE` | `UI_NAV_POP_RESET` | C4, 내부 |
| `HS_STATS_FARM_DROP_INIT` | `AGENT_STATS_FARMDROP_INIT` | C4. 코드확인: Agent 생성자 stats 필드 초기화. UI_NAV_→**AGENT_** 귀속 수정(27.22) |
| `HS_AGGVIZ_UPDATE_MOVED` | `AGENT_AGGVIZ_UPDATE` | C4. 코드확인: Agent.think() 내 공격성 viz 히스테리시스. UI_NAV_→**AGENT_** 귀속 수정(27.22) |
| `HS_AGENT_UPDATE` | `UI_AGENT_UPDATE` | C4 |
| `HS_FITTER_P1_1_PATH2D_CACHE` | `RENDER_PATH2D_CACHE` | → **M11** 이관 귀속 수정(27.22) 참조 |
| `HS_FITTER_P1_1_AGENT_TRI_REPLACE` | `AGENT_DRAW_TRI` | → **M11** 이관 귀속 수정(27.22) 참조 |
| `UI_INSPECTOR_PANEL_UPDATE` | 유지 | |
| `HS_INSPECTOR_SUBST_FARM_DROP_ROW` | `UI_INSPECTOR_FARM_DROP_ROW` | C4, 내부. **HTML 주석 형식** — 수동 처리 필요. Phase 3-B |
| `UI_SYSTEM_PANELS_UPDATE` (GAP-F) | 유지 | 신설 예정 |
| `HS_FARM_OBS_MINI_BLOCKVIEW` | `UI_FARM_OBS_BLOCKVIEW` | C4 |
| `HS_MONITOR_CORE` | `UI_MONITOR_CORE` | C4 |
| `DEV_CONSOLE_HELPERS` | 유지 | 내부 |
| `HS_FITTER_P3_1_CORE_API` | `JS_CORE_API_FACADE` | C4. 코드확인: 전역 EvoSim.core facade(state+sim+render+ui). UI_→**JS_** 접두사 수정(27.22). JS_TOP 귀속 |
| `UI_DRAWERS_STEP1_INIT` | 유지 | 27.20 신설 |
| `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_LEGACY_STEP1` | `UI_DRAWERS_TOGGLERIGHT_STEP1` | C4. LEGACY 제거 시 코드 내 `// LEGACY: Step1 IIFE 경로` 주석 대체 필요. Phase 3-B |
| `UI_DRAWER_TOGGLES_CORE` | 유지 | |
| `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` | `UI_DRAWERS_TOGGLERIGHT` | C4. DIRTY 접미사 = 임시 더티플래그 상태, 제거 가능. Phase 3-B |
| `HS_FITTER_P1_3_TREND_TAB_DIRTY` | `UI_DRAWERS_TREND_TAB` | C4. DIRTY 접미사 동일. Phase 3-B |
| `UI_INPUT_COORD_UTILS` (UI_INPUT_WORLD_UTILS 분할 1) | 신설 | C2 분할, clientToWorld() — M12 |
| `UI_AGRO_HOVER_INPUT` (UI_INPUT_WORLD_UTILS 분할 2) | 신설 | C2 분할, updateAgroHoverFrom*() — M12 |
| `UI_CHIP_TABS_PNREV_INIT` (GAP-I) | 유지 | 신설 예정 |
| `HS_FITTER_P1_2_HISTORY_UI` | `UI_HISTORY_PANEL` | C4 |
| `HS_FITTER_P1_3_HISTORY_DRAW_CONTROL` | `UI_HISTORY_DRAW_CTRL` | C4, 내부 |
| `HS_FITTER_P1_1_PANELVISIBLE` | `UI_PANEL_VISIBLE` | C4 |

### M13 — `ui_init.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `UI_DOM` | 유지 | HTML 구조 전체 |
| `HOME_OVERLAY` | 유지 | 내부 |
| `UI_HOME` | 유지 | 내부 |
| `CONFIG_OVERLAY` | 유지 | 내부 |
| `UI_CONFIG` | 유지 | 내부 |
| `UI_INGAME` | 유지 | 내부 |
| `UI_STARTUP_CONTROLS_INIT` | 유지 | 27.20 신설 |
| `CONTROL_DOCK_STEP2A_INIT` | 유지 | 내부 |
| `HS_FITTER_P4_1_INPUT_BINDINGS` | `UI_INPUT_BINDINGS` | C4 |
| `HS_HOTKEYS` | `UI_HOTKEYS` | C4, 내부 |
| `HS_A3_KEYP_REPLACE` | `UI_HOTKEYS_KEYP` | C4, 내부 |

---

## 3. Phase 1 점검 선결 항목 — **확정 완료** (2026-03-07)

| 블록명 | 확정 귀속 | 확정 조치 |
|--------|---------|---------|
| `SIM_GLOBALS_POST_GAIA_ENV30` | **M10** | 코드 직접 확인: terrain상수/엔티티프로필/전투함수/food앵커/시뮬상태 전역 — social 성격 0. C2 분할은 Phase 3-B |
| `ATLAS_P1_*` 4개 | **M11** | 코드 직접 확인: bgCtx 픽셀쓰기/drawImage/Canvas 2D 전용. C3 청사진 기록, 코드이동은 TS 후 |
| `FOOD_PHASE0_SIM_UTILS` (GAP-G) | **M10** | 코드 직접 확인: setInterval+루프타이밍+전엔티티그리드캐시+profiler. 명칭 → `SIM_LOOP_INIT_VARS` |
| `UI_INPUT_WORLD_UTILS` | **M12** | 코드 직접 확인: C2 분할 → `UI_INPUT_COORD_UTILS`(clientToWorld) + `UI_AGRO_HOVER_INPUT`(updateAgroHoverFrom*) |

---

## 4. GAP 잔여 현황

| GAP | 확정 블록명 | 귀속 모듈 | 상태 |
|-----|-----------|---------|------|
| GAP-A | `TS_SOCIAL_VOICE_GLOBALS` | M09 | 신설 예정 |
| GAP-D | `FOOD_GRID_CORE` | M08 | 신설 예정 |
| GAP-E | `BRAIN_CANVAS_DRAW` | M11 | 신설 예정 |
| GAP-F | `UI_SYSTEM_PANELS_UPDATE` | M12 | 신설 예정 |
| GAP-G | `SIM_LOOP_INIT_VARS` | M10 | 신설 예정 (명칭 확정: setInterval+루프타이밍+전엔티티그리드캐시+profiler) |
| GAP-I | `UI_CHIP_TABS_PNREV_INIT` | M12 | 신설 예정 |

---

## 5. 네이밍 컨벤션 확정본

| 패턴 | 적용 맥락 | 예시 |
|------|---------|------|
| `SUBSYSTEM_ROLE` | 신규 블록 및 HS_ rename 대상 | `FOOD_GRID_CORE`, `SIM_RAF_SINGLETON` |
| `SIM_COLLISION_LAYER_TYPE` | sim 내 collision 하위 블록 | `SIM_COLLISION_AGENT_FIGHT` |
| `[HS-origin: <원래명>]` | HS_ rename 시 블록 내 주석 | `[HS-origin: HS_COLLISION_LOOP]` |
| `// TS 귀속: SWEEP/COMMIT` | TS 경계 명시 블록 | `SIM_COLLISION_BIRTH_PEND` |
| `// depends: <블록명> — needs fix` | TS 전 선결 의존성 | `SIM_COLLISION_PREDATOR` |
| `// LEGACY: <설명>` | LEGACY 접미사 제거 시 대체 주석 | `// LEGACY: Step1 IIFE 경로` |

---

## 6. 보류 트래킹 (v27.22 갱신 / 잔여 11개)

> 3-B-1(v27.22) 완료 후 잔여 보류 목록. 3-B-2 검증 결과 반영.

### 3-B-2 대상 (코드 검증 완료 — 즉시 실행 가능)

| 블록명 | 확정명 | 상태 |
|--------|--------|------|
| `HS_FITTER_P4_1_UITICK_REGION` | `UI_UITICK_REGION` | 3-B-2 실행 예정 |
| `HS_UITICK_WRAPPERS` | `UI_UITICK_WRAPPERS` | 3-B-2 실행 예정 |
| `HS_FITTER_P1_4_UITICK500_ROUTER` | `UI_UITICK500_ROUTER` | 3-B-2 실행 예정 |
| `HS_FITTER_P1_1_RENDERLOGS_GUARD` | `SPAWN_RENDERLOGS_GUARD` | 3-B-2 실행 예정 |
| `HS_19_CLAMOR_RESET_ON_START_INSERT` | `UI_NAV_CLAMOR_RESET` | 3-B-2 실행 예정 |
| `HS_A3_POP_RESET_REPLACE` | `UI_NAV_POP_RESET` | 3-B-2 실행 예정 |
| `HS_FITTER_P1_1_PATH2D_CACHE` | `RENDER_PATH2D_CACHE` | 3-B-2 실행 예정 (M11 귀속) |
| `HS_STATS_FARM_DROP_INIT` | `AGENT_STATS_FARMDROP_INIT` | 3-B-2 실행 예정 |
| `HS_AGGVIZ_UPDATE_MOVED` | `AGENT_AGGVIZ_UPDATE` | 3-B-2 실행 예정 |
| `HS_FITTER_P1_1_AGENT_TRI_REPLACE` | `AGENT_DRAW_TRI` | 3-B-2 실행 예정 (M11 귀속) |
| `HS_FITTER_P3_1_CORE_API` | `JS_CORE_API_FACADE` | 3-B-2 실행 예정 (JS_TOP 귀속) |

### 3-B-3 대상 (특수 처리)

| 블록명 | 확정명 | 보류 이유 | 재검토 조건 |
|--------|--------|---------|-----------|
| `HS_INSPECTOR_SUBST_FARM_DROP_ROW` | `UI_INSPECTOR_FARM_DROP_ROW` | HTML 주석 형식 — 수동 Edit | Phase 3-B-3 |
| `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_LEGACY_STEP1` | `UI_DRAWERS_TOGGLERIGHT_STEP1` | LEGACY 제거 시 `// LEGACY:` 주석 대체 필요 | Phase 3-B-3 |

### Phase 3-C/D 대상

| 블록명 | 확정명 | 보류 이유 | 재검토 조건 |
|--------|--------|---------|-----------|
| `HS_COLLISION_LOOP` | `SIM_COLLISION_CORE` | 6개 하위블록 분할 선결 | Phase 3-C 분할 실행 후 |
| `HS_PHASE1_HELPERS` | `SIM_PHASE1_HELPERS` | ISS-05 3분할 권장 | ISS-05 분할 확정 후 |
| `HS_AGRO_SELECTED_TILE_LAZY_TOUCH` | *(유지)* | 청사진 유지 정책 | — |

---

## 7. 후속 로드맵

| 단계 | 내용 | 예상 버전 |
|------|------|---------|
| Phase 3-B-1 | 7개 rename (M10 2개 + M12 5개) | v27.22 ✓ 완료 |
| Phase 3-B-2 | 11개 rename (코드검증 완료) | v27.23 |
| Phase 3-B-3 | 2개 특수처리 (HTML 형식 수동 + LEGACY 주석) | v27.24 |
| Phase 3-C | HS_COLLISION_LOOP → 6개 하위블록 실질 분할 (TS 선결 의존성 조율) | v27.25 |
| Phase 3-D | ISS-05 HS_PHASE1_HELPERS 3분할 | v27.26 |
