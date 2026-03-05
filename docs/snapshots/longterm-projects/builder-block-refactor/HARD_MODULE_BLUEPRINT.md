# HARD_MODULE_BLUEPRINT — EvoSim 하드모듈화 청사진

**작성일:** 2026-03-05
**기준 버전:** v27.20_fix1
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
| HS_ rename | 가능. 블록 내 `[HS-origin: <원래명>]` 주석 한 줄 보존 |
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
| `UI_INPUT_WORLD_UTILS` | 유지 | ⚠️ UI 성격이나 food 조작 유틸, 점검 필요 |
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
| `SIM_GLOBALS_POST_GAIA_ENV30` | ⚠️ 미확정 | 내용 미확인 — Phase 1 점검 최우선 |

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
| `HS_FITTER_P4_1_UITICK_REGION` | `SIM_UITICK_REGION` | C4 |
| `HS_UITICK_WRAPPERS` | `SIM_UITICK_WRAPPERS` | C4, 내부 |
| `HS_FITTER_P1_4_UITICK500_ROUTER` | `SIM_UITICK500_ROUTER` | C4, 내부 |
| `ATLAS_P1_OVERVIEW_VARS` | 유지 | ⚠️ render 귀속 가능성, 점검 필요 |
| `ATLAS_P1_STATICMAP_CLIP` | 유지 | ⚠️ render 귀속 가능성, 점검 필요 |
| `ATLAS_P1_OVERVIEW_CALL` | 유지 | 내부 |
| `ATLAS_P1_OVERVIEW_FN` | 유지 | |
| `HS_A3_POP_RING_REPLACE` | `POP_RING_CORE` | C4 |
| `HS_FITTER_P1_2_POP_SAMPLE` | `POP_SAMPLE_CORE` | C4, 내부 |

### M11 — `render.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `HS_FITTER_P4_1_RENDER_LOOP` | `RENDER_LOOP_CORE` | C4 |
| `BRAIN_CANVAS_DRAW` (GAP-E) | 유지 | 신설 예정 |
| `CANVAS_CONTEXT_INIT` | 유지 | 27.20 신설 |
| `AGENT_DRAW_TAIL` | 유지 | 27.20 신설 |

### M12 — `ui.js`
| 현재 블록명 | 확정 블록명 | 비고 |
|------------|-----------|------|
| `UI_NAV_ROUTING` | 유지 | ISS-02: 3분할 권장 유지 |
| `HS_19_CLAMOR_RESET_ON_START_INSERT` | `UI_NAV_CLAMOR_RESET` | C4, 내부 |
| `HS_A3_POP_RESET_REPLACE` | `UI_NAV_POP_RESET` | C4, 내부 |
| `HS_FITTER_P1_1_PATH2D_CACHE` | `UI_NAV_PATH2D_CACHE` | C4, 내부 |
| `HS_STATS_FARM_DROP_INIT` | `UI_NAV_STATS_FARM_INIT` | C4, 내부 |
| `HS_AGGVIZ_UPDATE_MOVED` | `UI_NAV_AGGVIZ_UPDATE` | C4, 내부 |
| `HS_AGENT_UPDATE` | `UI_AGENT_UPDATE` | C4 |
| `HS_FITTER_P1_1_AGENT_TRI_REPLACE` | `UI_AGENT_TRI_REPLACE` | C4 |
| `UI_INSPECTOR_PANEL_UPDATE` | 유지 | |
| `HS_INSPECTOR_SUBST_FARM_DROP_ROW` | `UI_INSPECTOR_FARM_DROP_ROW` | C4, 내부 |
| `UI_SYSTEM_PANELS_UPDATE` (GAP-F) | 유지 | 신설 예정 |
| `HS_FARM_OBS_MINI_BLOCKVIEW` | `UI_FARM_OBS_BLOCKVIEW` | C4 |
| `HS_MONITOR_CORE` | `UI_MONITOR_CORE` | C4 |
| `DEV_CONSOLE_HELPERS` | 유지 | 내부 |
| `HS_FITTER_P3_1_CORE_API` | `UI_CORE_API` | C4 |
| `UI_DRAWERS_STEP1_INIT` | 유지 | 27.20 신설 |
| `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_LEGACY_STEP1` | `UI_DRAWERS_TOGGLERIGHT_STEP1` | C4 |
| `UI_DRAWER_TOGGLES_CORE` | 유지 | |
| `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` | `UI_DRAWERS_TOGGLERIGHT` | C4 |
| `HS_FITTER_P1_3_TREND_TAB_DIRTY` | `UI_DRAWERS_TREND_TAB` | C4 |
| `UI_CHIP_TABS_PNREV_INIT` (GAP-I) | 유지 | 신설 예정 |
| `FOOD_PHASE0_SIM_UTILS` (GAP-G) | 유지 | ⚠️ sim/food 귀속 재검토 필요 |
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

## 3. Phase 1 점검 선결 항목 (⚠️)

| 블록명 | 이슈 | 귀속 후보 |
|--------|------|---------|
| `SIM_GLOBALS_POST_GAIA_ENV30` | 586줄, 내용 미확인. 명칭상 전역변수 초기화 | M09(social) 또는 M10(sim) |
| `ATLAS_P1_*` 3개 | render 성격 가능성 | M10(sim) vs M11(render) |
| `FOOD_PHASE0_SIM_UTILS` (GAP-G) | food/sim 혼합 코드 | M08(food) vs M10(sim) |
| `UI_INPUT_WORLD_UTILS` | 명칭은 UI이나 food 조작 유틸 가능성 | M08(food) vs M12(ui) |

---

## 4. GAP 잔여 현황

| GAP | 확정 블록명 | 귀속 모듈 | 상태 |
|-----|-----------|---------|------|
| GAP-A | `TS_SOCIAL_VOICE_GLOBALS` | M09 | 신설 예정 |
| GAP-D | `FOOD_GRID_CORE` | M08 | 신설 예정 |
| GAP-E | `BRAIN_CANVAS_DRAW` | M11 | 신설 예정 |
| GAP-F | `UI_SYSTEM_PANELS_UPDATE` | M12 | 신설 예정 |
| GAP-G | `FOOD_PHASE0_SIM_UTILS` | ⚠️ 미확정 | 신설 예정 |
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
