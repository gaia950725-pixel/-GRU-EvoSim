# BUILDER_BLOCK Snapshot — EvoSim index.html

**최초 작성일:** 2026-03-04
**최종 갱신일:** 2026-03-04 (v27.11 라인번호 재동기화 · 감사 대응 · 기획안 추가)
**대상 파일:** `index.html`
**기준 버전:** EvoSim 27.11 (boot-init strip-yield finalize-reorder)
**총 BUILDER_BLOCK 마커 수:** 214개 (START/END 쌍 포함)

> **감사(AUDIT) 반영 노트** — `BUILDER_BLOCK_SNAPSHOT_AUDIT.md` (Codex, 2026-03-04) 지적 사항:
> - v27.10 스냅샷 대비 9,700라인 이후 **48개 블록에서 +5~+6 라인 시프트** 발생 (v27.11 패치 삽입 영향)
> - 본 버전에서 전체 라인번호를 v27.11 기준으로 재생성함
> - 중복 블록명 `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` (2개소) 이슈 유지·기록

---

## 목차

1. [전체 블록 목록](#1-전체-블록-목록)
2. [블록 계층 구조](#2-블록-계층-구조)
3. [누락 구간 상세 분석](#3-누락-구간-상세-분석)
4. [구획화 기획안](#4-구획화-기획안)
5. [부록: 이슈 목록](#5-부록-이슈-목록)

---

## 1. 전체 블록 목록

라인번호는 **v27.11 `index.html`** 기준. START 마커 라인 기준 정렬.

| # | 블록명 | START | END | 규모(줄) | 계층 |
|---|--------|-------|-----|---------|------|
| 1 | `STYLES` | 7 | 1713 | ~1706 | 최상위 |
| 2 | `CSS` | 9 | 1484 | ~1475 | STYLES 내 |
| 3 | `UI_DOM` | 1767 | 18516 | ~16749 | 최상위 |
| 4 | `HOME_OVERLAY` | 1788 | 1804 | ~16 | UI_DOM 내 |
| 5 | `UI_HOME` | 1789 | 1803 | ~14 | HOME_OVERLAY 내 |
| 6 | `CONFIG_OVERLAY` | 1806 | 2182 | ~376 | UI_DOM 내 |
| 7 | `UI_CONFIG` | 1807 | 2180 | ~373 | CONFIG_OVERLAY 내 |
| 8 | `UI_INGAME` | 2185 | 2361 | ~176 | UI_DOM 내 |
| 9 | `ARCHITECTURE_OVERVIEW` | 2428 | 2466 | ~38 | 독립(설계 주석) |
| 10 | `JS_TOP` | 2468 | 2472 | ~4 | 최상위 JS |
| 11 | `JS_BOOTSTRAP_CORE` | 2473 | 2863 | ~390 | 최상위 JS |
| 12 | `SURNAME_AUTO_CORE` | 2864 | 3284 | ~420 | 최상위 JS |
| 13 | `SURNAME_AUTO_SANITIZE_CLAN` | 3168 | 3183 | ~15 | SURNAME_AUTO_CORE 내 |
| 14 | `WORLDGEN_UI_INIT` | 3288 | 3748 | ~460 | 최상위 JS |
| 15 | `CONFIG_TABS` | 3462 | 3578 | ~116 | WORLDGEN_UI_INIT 내 |
| 16 | `HS_FITTER_P1_3_GRAPH_FLAGS` | 3697 | 3700 | ~3 | WORLDGEN_UI_INIT 내 |
| 17 | `HS_FITTER_P1_3A_HISTORY_ADAPTER` | 3701 | 3714 | ~13 | WORLDGEN_UI_INIT 내 |
| 18 | `HS_FITTER_P1_3_PANEL_EXPAND_DIRTY` | 3737 | 3743 | ~6 | WORLDGEN_UI_INIT 내 |
| 19 | `CFG_DEFAULTS` | 3750 | 3998 | ~248 | 최상위 JS |
| 20 | `NN_IO_ENUMS` | 3999 | 4108 | ~109 | 최상위 JS |
| 21 | `VISION_CACHE_DERIVED` | 4109 | 4125 | ~16 | 최상위 JS |
| 22 | `HS_OCCLUSION_21RAY_GLOBALS` | 4127 | 4241 | ~114 | 최상위 JS |
| 23 | `AGRO_GLOBALS` | 4242 | 4253 | ~11 | 최상위 JS |
| 24 | `HS_AGRO_BLOCKVIEW_UTILS` | 4262 | 4448 | ~186 | 최상위 JS |
| 25 | `UI_INPUT_WORLD_UTILS` | 4452 | 4483 | ~31 | 최상위 JS |
| 26 | `HS_AGRO_TOUCH_REPIDX` | 4487 | 4489 | ~2 | 최상위 JS |
| 27 | `HS_AGRO_PEEK_TILE` | 4520 | 4541 | ~21 | 최상위 JS |
| 28 | `HS_AGRO_TOP4_REPIDX` | 4548 | 4550 | ~2 | 최상위 JS |
| 29 | `HS_AGRO_HARVEST` | 4608 | 4729 | ~121 | 최상위 JS |
| 30 | `HS_AGRO_HARVEST_REPIDX` | 4611 | 4613 | ~2 | HS_AGRO_HARVEST 내 |
| 31 | `HS_AGRO_HARVEST_FARMDROP` | 4658 | 4712 | ~54 | HS_AGRO_HARVEST 내 |
| 32 | `AGRO_FARM_WORK_TICK` | 4732 | 4834 | ~102 | 최상위 JS |
| 33 | `HS_AGRO_WORK_REPIDX` | 4740 | 4743 | ~3 | AGRO_FARM_WORK_TICK 내 |
| 34 | `CORE_MATH_ENERGY_UTILS` | 4836 | 5031 | ~195 | 최상위 JS |
| 35 | `AGRO_INIT_CORE` | 5032 | 5322 | ~290 | 최상위 JS |
| 36 | `HS_INITAGRO_FARMBUILD_BLOCKS` | 5094 | 5243 | ~149 | AGRO_INIT_CORE 내 |
| 37 | `HS_INITAGRO_FARMSUIT_BLOCKS` | 5269 | 5320 | ~51 | AGRO_INIT_CORE 내 |
| 38 | `GAIA_30PX_LOOKUP_APIS` | 5327 | 5479 | ~152 | 최상위 JS |
| 39 | `TIME_NARRATIVE_GLOBALS` | 5481 | 5566 | ~85 | 최상위 JS |
| 40 | `MAP_PRESETS` | 5568 | 5654 | ~86 | 최상위 JS |
| 41 | `GAIA_SOT_CORE` | 5659 | 6070 | ~411 | 최상위 JS |
| 42 | `GAIA_MOUNTAINS` | 6072 | 6543 | ~471 | 최상위 JS |
| 43 | `GAIA_MTN_POLICY` | 6081 | 6092 | ~11 | GAIA_MOUNTAINS 내 |
| 44 | `GAIA_ENV30` | 6545 | 7123 | ~578 | 최상위 JS |
| 45 | `SIM_GLOBALS_POST_GAIA_ENV30` | 7124 | 7710 | ~586 | 최상위 JS |
| 46 | `HS_19_CLAMOR_CORE_ADD` | 7712 | 8036 | ~324 | 최상위 JS |
| — | *(GAP-A)* | 8037 | 8093 | ~57 | **블록 없음** |
| 47 | `AGENT_GRID_CORE` | 8094 | 9079 | ~985 | 최상위 JS |
| 48 | `HS_18_7_GLOBALS` | 8098 | 8105 | ~7 | AGENT_GRID_CORE 내 |
| 49 | `HS_18_7_BUILD_FUNC_REPLACE` | 8122 | 8197 | ~75 | AGENT_GRID_CORE 내 |
| 50 | `GRID_QUERY_API` | 9080 | 9118 | ~38 | 최상위 JS |
| 51 | `HS_QUERY_API` | 9084 | 9116 | ~32 | GRID_QUERY_API 내 |
| 52 | `HS_18_7_ADD_AGENT_REPLACE` | 9128 | 9151 | ~23 | 최상위 JS |
| 53 | `GRIDDBG_HELPERS` | 9152 | 9224 | ~72 | 최상위 JS |
| 54 | `HERB_GRID_CORE` | 9225 | 9348 | ~123 | 최상위 JS |
| 55 | `HS_A2_HERBGRID_GLOBALS` | 9246 | 9253 | ~7 | HERB_GRID_CORE 내 |
| 56 | `HS_A2_BUILD_HERBGRID_REPLACE` | 9274 | 9331 | ~57 | HERB_GRID_CORE 내 |
| 57 | `PRED_GRID_CORE` | 9349 | 9478 | ~129 | 최상위 JS |
| — | *(GAP-B)* | 9479 | 9716 | ~238 | **블록 없음** |
| 58 | `UI_NAV_ROUTING` | 9717 | 11469 | ~1752 | 최상위 JS (대형) |
| 59 | `UMC_LEGACY_WRAPPERS_REMOVED` | 9898 | 9904 | ~6 | UI_NAV_ROUTING 내 |
| 60 | `HS_19_CLAMOR_RESET_ON_START_INSERT` | 10444 | 10466 | ~22 | UI_NAV_ROUTING 내 |
| 61 | `HS_A3_POP_RESET_REPLACE` | 10470 | 10473 | ~3 | UI_NAV_ROUTING 내 |
| 62 | `HS_FITTER_P1_1_PATH2D_CACHE` | 10810 | 10818 | ~8 | UI_NAV_ROUTING 내 |
| 63 | `HS_STATS_FARM_DROP_INIT` | 10877 | 10879 | ~2 | UI_NAV_ROUTING 내 |
| 64 | `HS_AGGVIZ_UPDATE_MOVED` | 11084 | 11104 | ~20 | UI_NAV_ROUTING 내 |
| 65 | `HS_AGENT_UPDATE` | 11471 | 11569 | ~98 | 최상위 JS |
| 66 | `HS_FITTER_P1_1_AGENT_TRI_REPLACE` | 11686 | 11713 | ~27 | 최상위 JS |
| — | *(GAP-C ★)* | 11714 | 12403 | ~690 | **블록 없음** |
| 67 | `ATLAS_P1_OVERVIEW_VARS` | 12404 | 12409 | ~5 | 최상위 JS |
| 68 | `HS_A3_POP_RING_REPLACE` | 12492 | 12576 | ~84 | 최상위 JS |
| 69 | `HS_FITTER_P1_2_POP_SAMPLE` | 12570 | 12575 | ~5 | HS_A3_POP_RING_REPLACE 내 |
| 70 | `ATLAS_P1_STATICMAP_CLIP` | 12608 | 13171 | ~563 | 최상위 JS |
| 71 | `ATLAS_P1_OVERVIEW_CALL` | 13194 | 13197 | ~3 | 최상위 JS |
| 72 | `ATLAS_P1_OVERVIEW_FN` | 13200 | 13222 | ~22 | 최상위 JS |
| 73 | `SPAWN_AND_LOG_CORE` | 13224 | 13599 | ~375 | 최상위 JS |
| 74 | `HS_FITTER_P1_1_RENDERLOGS_GUARD` | 13578 | 13583 | ~5 | SPAWN_AND_LOG_CORE 내 |
| — | *(GAP-D)* | 13600 | 13714 | ~115 | **블록 없음** |
| 75 | `HS_COLLISION_LOOP` | 13715 | 14726 | ~1011 | 최상위 JS (대형) |
| 76 | `HS_APPLYEAT_FARM_DROP_STATS` | 14457 | 14462 | ~5 | HS_COLLISION_LOOP 내 |
| 77 | `HS_NAT_AREA_SCALE` | 14493 | 14496 | ~3 | HS_COLLISION_LOOP 내 |
| — | *(GAP-E)* | 14727 | 14828 | ~102 | **블록 없음** |
| 78 | `HS_FITTER_P4_1_INPUT_BINDINGS` | 14829 | 15130 | ~301 | 최상위 JS |
| 79 | `HS_HOTKEYS` | 14841 | 14984 | ~143 | HS_FITTER_P4_1_INPUT_BINDINGS 내 |
| 80 | `HS_A3_KEYP_REPLACE` | 14965 | 14982 | ~17 | HS_HOTKEYS 내 |
| 81 | `ERA_BTN_SURNAME_AUTO_WIRE` | 15133 | 15185 | ~52 | 최상위 JS |
| — | *(GAP-F)* | 15186 | 15335 | ~150 | **블록 없음** |
| 82 | `HS_FARM_OBS_MINI_BLOCKVIEW` | 15336 | 15486 | ~150 | 최상위 JS |
| 83 | `HS_BRAIN_THROTTLE_HELPER` | 15509 | 15546 | ~37 | 최상위 JS |
| 84 | `UI_INSPECTOR_PANEL_UPDATE` | 15548 | 15708 | ~160 | 최상위 JS |
| 85 | `HS_INSPECTOR_SUBST_FARM_DROP_ROW` | 15640 | 15642 | ~2 | UI_INSPECTOR_PANEL_UPDATE 내 |
| 86 | `HS_FITTER_P4_1_UITICK_REGION` | 15710 | 15735 | ~25 | 최상위 JS |
| 87 | `HS_UITICK_WRAPPERS` | 15712 | 15734 | ~22 | HS_FITTER_P4_1_UITICK_REGION 내 |
| 88 | `HS_FITTER_P1_4_UITICK500_ROUTER` | 15713 | 15729 | ~16 | HS_UITICK_WRAPPERS 내 |
| 89 | `HS_FITTER_P1_1_PANELVISIBLE` | 15737 | 15747 | ~10 | 최상위 JS |
| 90 | `HS_FITTER_P1_2_HISTORY_UI` | 15749 | 15789 | ~40 | 최상위 JS |
| 91 | `HS_FITTER_P1_3_HISTORY_DRAW_CONTROL` | 15765 | 15784 | ~19 | HS_FITTER_P1_2_HISTORY_UI 내 |
| — | *(GAP-G)* | 15790 | 15875 | ~86 | **블록 없음** |
| 92 | `HS_A3_DEV_PERF_API` | 15876 | 15921 | ~45 | 최상위 JS |
| 93 | `HS_FITTER_P4_1_RENDER_LOOP` | 15941 | 16195 | ~254 | 최상위 JS |
| 94 | `HS_PHASE1_HELPERS` | 16200 | 17093 | ~893 | 최상위 JS (대형) |
| 95 | `HS_FITTER_P4_1_SIM_LOOP` | 16201 | 16890 | ~689 | HS_PHASE1_HELPERS 내 |
| 96 | `HS_19_SIMLOOP_DECAY_INSERT` | 16376 | 16382 | ~6 | HS_FITTER_P4_1_SIM_LOOP 내 |
| 97 | `HS_AGRO_SELECTED_TILE_LAZY_TOUCH` | 16384 | 16398 | ~14 | HS_FITTER_P4_1_SIM_LOOP 내 |
| 98 | `AUTOCLAN_ANIMATE_HOOK` | 16399 | 16407 | ~8 | HS_FITTER_P4_1_SIM_LOOP 내 |
| 99 | `HS_RAF_SINGLETON_21_1` | 17095 | 17126 | ~31 | 최상위 JS |
| 100 | `HS_DISPOSE_WORLD_21_1` | 17128 | 17191 | ~63 | 최상위 JS |
| 101 | `HS_MONITOR_CORE` | 17222 | 17829 | ~607 | 최상위 JS |
| 102 | `DEV_CONSOLE_HELPERS` | 17474 | 17657 | ~183 | HS_MONITOR_CORE 내 |
| 103 | `HS_FITTER_P3_1_CORE_API` | 17891 | 17922 | ~31 | 최상위 JS |
| — | *(GAP-H)* | 17923 | 18081 | ~159 | **블록 없음** |
| 104 | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` *(1st)* | 18082 | 18087 | ~5 | UI_DRAWERS IIFE 내 |
| 105 | `UI_DRAWER_TOGGLES_CORE` | 18214 | 18396 | ~182 | 최상위 JS |
| 106 | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` *(2nd)* | 18239 | 18244 | ~5 | UI_DRAWER_TOGGLES_CORE 내 |
| — | *(GAP-I)* | 18397 | 18453 | ~57 | **블록 없음** |
| 107 | `HS_FITTER_P1_3_TREND_TAB_DIRTY` | 18454 | 18463 | ~9 | 최상위 JS |

---

## 2. 블록 계층 구조

```
index.html (18,516줄 / v27.11)
├── STYLES (7–1713)
│   └── CSS (9–1484)
│
├── UI_DOM (1767–18516)                       ─── 최상위 HTML
│   ├── HOME_OVERLAY (1788–1804)
│   │   └── UI_HOME (1789–1803)
│   ├── CONFIG_OVERLAY (1806–2182)
│   │   └── UI_CONFIG (1807–2180)
│   └── UI_INGAME (2185–2361)
│
│   [<script> JS 구간 시작 ~2420]
│
├── ARCHITECTURE_OVERVIEW (2428–2466)          ←── 설계 주석
├── JS_TOP (2468–2472)                         ←── import · 전역 선언
├── JS_BOOTSTRAP_CORE (2473–2863)             ←── 초기화 코어
│
├── SURNAME_AUTO_CORE (2864–3284)
│   └── SURNAME_AUTO_SANITIZE_CLAN (3168–3183)
│
├── WORLDGEN_UI_INIT (3288–3748)              ←── 월드 생성 UI
│   ├── CONFIG_TABS (3462–3578)
│   ├── HS_FITTER_P1_3_GRAPH_FLAGS (3697–3700)
│   ├── HS_FITTER_P1_3A_HISTORY_ADAPTER (3701–3714)
│   └── HS_FITTER_P1_3_PANEL_EXPAND_DIRTY (3737–3743)
│
├── CFG_DEFAULTS (3750–3998)                  ←── 시뮬레이션 설정
├── NN_IO_ENUMS (3999–4108)                   ←── NN 슬롯 열거
├── VISION_CACHE_DERIVED (4109–4125)
├── HS_OCCLUSION_21RAY_GLOBALS (4127–4241)    ←── 시야 차폐 전역
├── AGRO_GLOBALS (4242–4253)
│
├── HS_AGRO_BLOCKVIEW_UTILS (4262–4448)
├── UI_INPUT_WORLD_UTILS (4452–4483)
├── HS_AGRO_TOUCH_REPIDX (4487–4489)
├── HS_AGRO_PEEK_TILE (4520–4541)
├── HS_AGRO_TOP4_REPIDX (4548–4550)
│
├── HS_AGRO_HARVEST (4608–4729)
│   ├── HS_AGRO_HARVEST_REPIDX (4611–4613)
│   └── HS_AGRO_HARVEST_FARMDROP (4658–4712)
│
├── AGRO_FARM_WORK_TICK (4732–4834)
│   └── HS_AGRO_WORK_REPIDX (4740–4743)
│
├── CORE_MATH_ENERGY_UTILS (4836–5031)        ←── 에너지·수학 유틸
│
├── AGRO_INIT_CORE (5032–5322)
│   ├── HS_INITAGRO_FARMBUILD_BLOCKS (5094–5243)
│   └── HS_INITAGRO_FARMSUIT_BLOCKS (5269–5320)
│
├── GAIA_30PX_LOOKUP_APIS (5327–5479)         ←── GAIA 30px 조회
├── TIME_NARRATIVE_GLOBALS (5481–5566)
├── MAP_PRESETS (5568–5654)
│
├── GAIA_SOT_CORE (5659–6070)                 ←── GAIA 단일진실원천
├── GAIA_MOUNTAINS (6072–6543)
│   └── GAIA_MTN_POLICY (6081–6092)
├── GAIA_ENV30 (6545–7123)                    ←── 기후·환경
├── SIM_GLOBALS_POST_GAIA_ENV30 (7124–7710)   ←── 시뮬 전역 (GAIA 이후)
│
├── HS_19_CLAMOR_CORE_ADD (7712–8036)         ←── 소음·음성 이벤트
│
│   ⚠ [GAP-A] 8037–8093 (~57줄)
│
├── AGENT_GRID_CORE (8094–9079)               ←── 에이전트 그리드 [985줄]
│   ├── HS_18_7_GLOBALS (8098–8105)
│   └── HS_18_7_BUILD_FUNC_REPLACE (8122–8197)
│
├── GRID_QUERY_API (9080–9118)
│   └── HS_QUERY_API (9084–9116)
├── HS_18_7_ADD_AGENT_REPLACE (9128–9151)
├── GRIDDBG_HELPERS (9152–9224)
│
├── HERB_GRID_CORE (9225–9348)
│   ├── HS_A2_HERBGRID_GLOBALS (9246–9253)
│   └── HS_A2_BUILD_HERBGRID_REPLACE (9274–9331)
│
├── PRED_GRID_CORE (9349–9478)
│
│   ⚠ [GAP-B] 9479–9716 (~238줄)
│
├── UI_NAV_ROUTING (9717–11469)               ←── UI 라우팅 [1752줄]
│   ├── UMC_LEGACY_WRAPPERS_REMOVED (9898–9904)
│   ├── HS_19_CLAMOR_RESET_ON_START_INSERT (10444–10466)
│   ├── HS_A3_POP_RESET_REPLACE (10470–10473)
│   ├── HS_FITTER_P1_1_PATH2D_CACHE (10810–10818)
│   ├── HS_STATS_FARM_DROP_INIT (10877–10879)
│   └── HS_AGGVIZ_UPDATE_MOVED (11084–11104)
│
├── HS_AGENT_UPDATE (11471–11569)
├── HS_FITTER_P1_1_AGENT_TRI_REPLACE (11686–11713)
│
│   ⚠ [GAP-C ★★] 11714–12403 (~690줄)  ← 최대 누락
│
├── ATLAS_P1_OVERVIEW_VARS (12404–12409)
├── HS_A3_POP_RING_REPLACE (12492–12576)
│   └── HS_FITTER_P1_2_POP_SAMPLE (12570–12575)
├── ATLAS_P1_STATICMAP_CLIP (12608–13171)
├── ATLAS_P1_OVERVIEW_CALL (13194–13197)
├── ATLAS_P1_OVERVIEW_FN (13200–13222)
│
├── SPAWN_AND_LOG_CORE (13224–13599)
│   └── HS_FITTER_P1_1_RENDERLOGS_GUARD (13578–13583)
│
│   ⚠ [GAP-D] 13600–13714 (~115줄)
│
├── HS_COLLISION_LOOP (13715–14726)           ←── 물리·충돌 루프 [1011줄]
│   ├── HS_APPLYEAT_FARM_DROP_STATS (14457–14462)
│   └── HS_NAT_AREA_SCALE (14493–14496)
│
│   ⚠ [GAP-E] 14727–14828 (~102줄)
│
├── HS_FITTER_P4_1_INPUT_BINDINGS (14829–15130)
│   └── HS_HOTKEYS (14841–14984)
│       └── HS_A3_KEYP_REPLACE (14965–14982)
│
├── ERA_BTN_SURNAME_AUTO_WIRE (15133–15185)
│
│   ⚠ [GAP-F] 15186–15335 (~150줄)
│
├── HS_FARM_OBS_MINI_BLOCKVIEW (15336–15486)
├── HS_BRAIN_THROTTLE_HELPER (15509–15546)
├── UI_INSPECTOR_PANEL_UPDATE (15548–15708)
│   └── HS_INSPECTOR_SUBST_FARM_DROP_ROW (15640–15642)
│
├── HS_FITTER_P4_1_UITICK_REGION (15710–15735)
│   └── HS_UITICK_WRAPPERS (15712–15734)
│       └── HS_FITTER_P1_4_UITICK500_ROUTER (15713–15729)
│
├── HS_FITTER_P1_1_PANELVISIBLE (15737–15747)
├── HS_FITTER_P1_2_HISTORY_UI (15749–15789)
│   └── HS_FITTER_P1_3_HISTORY_DRAW_CONTROL (15765–15784)
│
│   ⚠ [GAP-G] 15790–15875 (~86줄)
│
├── HS_A3_DEV_PERF_API (15876–15921)
├── HS_FITTER_P4_1_RENDER_LOOP (15941–16195)   ←── 렌더 루프
│
├── HS_PHASE1_HELPERS (16200–17093)            ←── Phase1 헬퍼 [893줄]
│   └── HS_FITTER_P4_1_SIM_LOOP (16201–16890) ←── 시뮬 메인 루프
│       ├── HS_19_SIMLOOP_DECAY_INSERT (16376–16382)
│       ├── HS_AGRO_SELECTED_TILE_LAZY_TOUCH (16384–16398)
│       └── AUTOCLAN_ANIMATE_HOOK (16399–16407)
│
├── HS_RAF_SINGLETON_21_1 (17095–17126)        ←── RAF 싱글턴
├── HS_DISPOSE_WORLD_21_1 (17128–17191)        ←── 월드 해제
│
├── HS_MONITOR_CORE (17222–17829)
│   └── DEV_CONSOLE_HELPERS (17474–17657)
│
├── HS_FITTER_P3_1_CORE_API (17891–17922)
│
│   ⚠ [GAP-H] 17923–18081 (~159줄)
│       └── [UI_DRAWERS Step1 IIFE — 미명명]
│
├── HS_FITTER_P1_3_TOGGLERIGHT_DIRTY [1st] (18082–18087)   ←── ⚠ 중복명
├── UI_DRAWER_TOGGLES_CORE (18214–18396)
│   └── HS_FITTER_P1_3_TOGGLERIGHT_DIRTY [2nd] (18239–18244)
│
│   ⚠ [GAP-I] 18397–18453 (~57줄)
│
└── HS_FITTER_P1_3_TREND_TAB_DIRTY (18454–18463)
```

---

## 3. 누락 구간 상세 분석

9개 GAP 구간 전체를 코드 내용 기준으로 분석함.
라인번호는 모두 **v27.11** 기준.

---

### GAP-A — 라인 8037–8093 (~57줄)

**경계:** `HS_19_CLAMOR_CORE_ADD_END` ↔ `AGENT_GRID_CORE_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 8038–8039 | `// [TS_PATCH 26.45]` 주석 · `const TS_CL_VOICE_EVENTS = []` 음성 이벤트 큐 전역 선언 |
| 8040–8055 | `function TS_evalShoutEveryTick(tick)` — 외향성 기반 외침 평가, 발성 이벤트 enqueue |
| 8056–8064 | `// [TS_PATCH 26.43]` 주석 · `let TS_solitarySkip = new Uint8Array(1024)` 외 솔리터리 스킵 typed array 전역 3종 선언 |
| 8065–8075 | `function TS_solSkipEnsureCap(n)` — 버퍼 확장 |
| 8076–8093 | `function TS_solSkipReset()` — 매 틱 초기화 |

**성격:** 소음(CLAMOR) 시스템과 에이전트 그리드 사이의 **사회·음성 행동 전역 브릿지**. 두 서브시스템 모두에 걸치기 때문에 경계 구간에 위치.

**추천 블록명:** `TS_SOCIAL_VOICE_GLOBALS` (57줄 단일 블록)

---

### GAP-B — 라인 9479–9716 (~238줄)

**경계:** `PRED_GRID_CORE_END` ↔ `UI_NAV_ROUTING_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 9479–9495 | `localStorage` 복원: `pred2_threatLevel`, `leaderMode` 설정값 |
| 9496–9530 | `speedSlider`, `regenSlider` addEventListener 이벤트 바인딩 |
| 9531–9545 | `pauseBtn` 클릭 핸들러 (paused 토글, 텍스트/색상 반영) |
| 9546–9620 | `function _cp_isTyping()`, `function initControlDockStep2A()` — Control Dock Step2-A 초기화 IIFE 본문 |
| 9621–9670 | `function setExpanded(on)`, `function toggleExpanded()`, resize listener, `btnExpand` 클릭 핸들러 |
| 9671–9716 | `function setTab(tab)`, tabWorld/tabTribe/chip `updateChips()` 초기화, `_benchBtn` 클릭 핸들러 |

**성격:** 그리드 시스템 완성 후 UI 컨트롤 독 초기화 구간. **그리드(자료구조) → UI(이벤트)** 전환 경계.

**추천 블록명:**
- 단일: `UI_STARTUP_CONTROLS_INIT` (238줄 단일)
- 분할: `LOCALSTORAGE_RESTORE` (9479–9495) + `SIM_SLIDER_BINDINGS` (9496–9545) + `CONTROL_DOCK_STEP2A_INIT` (9546–9716)

---

### GAP-C ★★ — 라인 11714–12403 (~690줄) 최대 누락

**경계:** `HS_FITTER_P1_1_AGENT_TRI_REPLACE_END` ↔ `ATLAS_P1_OVERVIEW_VARS_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 11714–12059 | `class Predator { ... }` — 포식자 개체 클래스 전체 (약 346줄) |
| ↳ 메서드 | `constructor`, `update(dt)`, `_applyHunger`, `_scanForPrey`, `_chaseAndAttack`, `_reproduce`, `draw(ctx, isSelected)` |
| 12060–12159 | `function _ss_applySolitaryActions(a, simulatedTicks)` — 개체 고독 행동 시뮬레이션 (약 100줄) |
| 12160–12403 | `class Herbivore { ... }` — 초식동물 개체 클래스 전체 (약 244줄) |
| ↳ 메서드 | `constructor`, `update(dt)`, `_applyHerbivoreHunger`, `_scanForFood`, `_scanForPredator`, `_reproduce`, `draw(ctx, isSelected)` |

**성격:** 가장 크고 핵심적인 개체 클래스 2종 + 솔리터리 행동 함수가 한데 묶여 있으나 블록이 전혀 없음. 에이전트 그리드(Agent) 이후에 위치하지만 **별도 Entity 계층** 취급이 적절.

**추천 블록명 (3분할):**
- `PREDATOR_CLASS_CORE` (11714–12059, ~346줄)
- `SOLITARY_ACTION_HELPERS` (12060–12159, ~100줄)
- `HERBIVORE_CLASS_CORE` (12160–12403, ~244줄)

---

### GAP-D — 라인 13600–13714 (~115줄)

**경계:** `SPAWN_AND_LOG_CORE_END` ↔ `HS_COLLISION_LOOP_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 13600–13611 | `function _ensureFoodGridDims()` — 먹이 그리드 차원 보장 |
| 13612–13640 | `class FoodGrid1D { constructor, add, remove, nearestInRadius, _compact }` — 1D 먹이 공간 인덱스 |
| 13641–13651 | `function _foodCellId(x, y)` — 셀 ID 계산 |
| 13652–13672 | `function _buildFoodGrid()`, `function _forNeighborFoodCells()` |
| 13673–13686 | `function _forFoodCellsInRadius(ax, ay, radius, fn)` |
| 13687–13700 | `function _compactFoodsIfNeeded()` |
| 13701–13714 | `function _inFront180_fromAngle()`, `function _inFront180_fromVel()` |

**성격:** 충돌 루프 직전에 위치한 **먹이 공간 인덱스 자료구조**. 물리 루프의 전제 조건.

**추천 블록명:** `FOOD_GRID_CORE` (13600–13714, 단일 115줄)

---

### GAP-E — 라인 14727–14828 (~102줄)

**경계:** `HS_COLLISION_LOOP_END` ↔ `HS_FITTER_P4_1_INPUT_BINDINGS_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 14727–14828 | `function drawBrain(ctx, brain)` — `brainCanvas` 신경망 시각화 전체 렌더링 (GRU 입출력, 레이어별 뉴런, 가중치 색상) |

**성격:** 물리 루프 직후, 입력 바인딩 직전에 단독으로 위치. brainCanvas 전용 렌더 함수 1개.

**추천 블록명:** `BRAIN_CANVAS_DRAW` (14727–14828, 단일 102줄)

---

### GAP-F — 라인 15186–15335 (~150줄)

**경계:** `ERA_BTN_SURNAME_AUTO_WIRE_END` ↔ `HS_FARM_OBS_MINI_BLOCKVIEW_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 15186–15335 | `function updateSystemPanels()` — 씨족 목록, 전쟁 상태, 가계도, 레전드 이벤트 패널 갱신 |
| ↳ 내부 섹션 | 씨족 인구 정렬 · 색상 배지 · 전쟁 카운터 · 역사 이벤트 테이블 행 생성 |

**성격:** 인스펙터와 별개로 존재하는 **사회/부족 시스템 전용 패널 업데이터**.

**추천 블록명:** `UI_SYSTEM_PANELS_UPDATE` (15186–15335, 단일 150줄)

---

### GAP-G — 라인 15790–15875 (~86줄)

**경계:** `HS_FITTER_P1_2_HISTORY_UI_END` ↔ `HS_A3_DEV_PERF_API_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 15790–15807 | `function _ensureFoodGridCurrent()` — 먹이 그리드 틱 동기화 보장 |
| 15808–15846 | Phase0 Policy (Grid/Death/Cache Unification) 관련 인라인 로직 — 에이전트 사망 시 그리드 정리 |
| 15847–15875 | `function _ssEnsureTickExt(n)` — 솔리터리 틱 익스텐션 배열 확장 보장 |

**성격:** 히스토리 UI와 퍼포먼스 API 사이에 끼어 있는 **시뮬 상태 유틸 클러스터**. Phase0 통합 정책 코드와 먹이 그리드 현행화 함수의 혼합.

**추천 블록명:** `FOOD_PHASE0_SIM_UTILS` (15790–15875, 단일 86줄)

---

### GAP-H — 라인 17923–18081 (~159줄)

**경계:** `HS_FITTER_P3_1_CORE_API_END` ↔ `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` (1st)

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 17923–17931 | `/* === UI DRAWERS: Step1 === */` 주석 헤더 + `(function __initUIDrawersStep1(){` IIFE 개시 |
| 17932–17956 | `isMobile()`, `_clampMode(v)`, `_readMode(key, defVal)` — 모바일·모드 유틸 |
| 17957–17989 | `loadSavedState()`, `saveState(st)`, `saveModes(st)` — localStorage 퍼시스턴스 |
| 17990–18025 | `updateOverlay()`, `syncToggleText(panelEl)` — 오버레이·토글 텍스트 동기화 |
| 18026–18055 | `apply()` — 전체 패널 상태 일괄 적용 |
| 18056–18075 | `cycleLeftMode()`, `cycleInspMode()`, `toggleRight()` — 패널 사이클·토글 액션 |
| 18076–18081 | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` (1st) 마커 — IIFE 내부 삽입 패치 조각 |

**성격:** 드로어 Step1 초기화 IIFE 전체가 블록 없이 존재. 내부에 패치 마커(TOGGLERIGHT_DIRTY)가 부분 삽입되어 있어 **블록 경계 혼란** 발생 원인.

**추천 블록명:** `UI_DRAWERS_STEP1_INIT` (17923–18081, 단일 159줄)
> 내부의 `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` (1st)는 서브블록으로 포함.

---

### GAP-I — 라인 18397–18453 (~57줄)

**경계:** `UI_DRAWER_TOGGLES_CORE_END` ↔ `HS_FITTER_P1_3_TREND_TAB_DIRTY_START`

**코드 내용 분해:**

| 범위 | 식별자 / 역할 |
|------|--------------|
| 18397–18400 | `/* === PNREV_P2: chip tabs init (Rev31 only) === */` 주석 헤더 + IIFE 개시 |
| 18401–18425 | Dashboard chipbar — `pnrev-dash-chipbar` 버튼 탭 초기화 · `apply(tab)` 함수 |
| 18426–18453 | Chronicle chipbar — `pnrev-chron-chipbar` 버튼 탭 초기화 · `apply(tab)` 함수 |

**성격:** PNREV_P2 칩탭 초기화 IIFE. `UI_DRAWER_TOGGLES_CORE`와 유사한 UI 초기화 패턴이나 별도 구분 없이 외부에 노출.

**추천 블록명:** `UI_CHIP_TABS_PNREV_INIT` (18397–18453, 단일 57줄)

---

## 4. 구획화 기획안

### 4-1. 기본 원칙

1. **비파괴 원칙**: 블록 마커는 주석만 삽입이며 코드 동작을 변경하지 않음.
2. **최소 단위**: 100줄 이상이면 반드시 최상위 블록으로 명명. 50줄 이상 논리 단위는 권장.
3. **계층 일관성**: 기존 네이밍 패턴(`SUBSYSTEM_ROLE` 또는 `HS_<phase>_<role>`) 준수.
4. **중복명 금지**: `_1st` / `_2nd` suffix 또는 블록 분리로 해결.

### 4-2. GAP 처리 우선순위 및 실행 계획

우선순위 = 규모 × 편집 충돌 위험 × 기능 중요도.

| 우선순위 | GAP | 규모 | 추천 블록명 | 단일/분할 | 작업 난이도 |
|---------|-----|------|------------|---------|-----------|
| **P0** | GAP-C | ~690줄 | `PREDATOR_CLASS_CORE` / `SOLITARY_ACTION_HELPERS` / `HERBIVORE_CLASS_CORE` | **3분할** | 중 (클래스 경계 명확) |
| **P1** | GAP-B | ~238줄 | `UI_STARTUP_CONTROLS_INIT` → 내부 3분할 | 단일+서브 | 중 (IIFE 경계 확인 필요) |
| **P1** | GAP-H | ~159줄 | `UI_DRAWERS_STEP1_INIT` | 단일 | 낮 (IIFE 1개) |
| **P2** | GAP-F | ~150줄 | `UI_SYSTEM_PANELS_UPDATE` | 단일 | 낮 (함수 1개) |
| **P2** | GAP-D | ~115줄 | `FOOD_GRID_CORE` | 단일 | 낮 (클래스+유틸 묶음) |
| **P3** | GAP-E | ~102줄 | `BRAIN_CANVAS_DRAW` | 단일 | 낮 (함수 1개) |
| **P3** | GAP-G | ~86줄 | `FOOD_PHASE0_SIM_UTILS` | 단일 | 중 (Phase0 정책 코드 혼합) |
| **P4** | GAP-A | ~57줄 | `TS_SOCIAL_VOICE_GLOBALS` | 단일 | 낮 |
| **P4** | GAP-I | ~57줄 | `UI_CHIP_TABS_PNREV_INIT` | 단일 | 낮 |

### 4-3. 대형 기존 블록 내부 분할 권장

기존 블록 중 1,000줄 내외 미분할 대형 블록 4개는 **별도 이슈**로 관리 권장.

| 블록명 | 현재 규모 | 분할 제안 |
|--------|---------|---------|
| `UI_NAV_ROUTING` | ~1752줄 | `UI_NAV_WORLDSTART_INIT` + `UI_NAV_SIM_INIT` + `UI_NAV_RENDER_INIT` 3분할 |
| `HS_COLLISION_LOOP` | ~1011줄 | `COLLISION_SPATIAL_QUERY` + `COLLISION_APPLY_EAT` + `COLLISION_APPLY_FIGHT` 3분할 |
| `AGENT_GRID_CORE` | ~985줄 | `AGENT_CLASS_CORE` + `AGENT_NN_SENSOR` + `AGENT_GENETICS_CORE` 3분할 |
| `HS_PHASE1_HELPERS` | ~893줄 | `PHASE1_PREFRAME_HELPERS` + 기존 `HS_FITTER_P4_1_SIM_LOOP` 2분할 (SIM_LOOP 이미 존재) |

### 4-4. 중복 블록명 정리

`HS_FITTER_P1_3_TOGGLERIGHT_DIRTY`가 두 위치에 존재 (18082, 18239). 해결 방안:

- **방안 A (권장):** GAP-H 작업 시 외부 IIFE를 `UI_DRAWERS_STEP1_INIT`으로 감싸고, 내부 마커는 `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_STEP1`로 rename.
- **방안 B:** 두 마커 모두 `_1` / `_2` suffix 추가 (`...DIRTY_1`, `...DIRTY_2`).
- **방안 C (최소변경):** AUDIT.md에 발생 원인 기록하고 다음 대형 리팩터까지 현상 유지.

### 4-5. 블록명 네이밍 컨벤션 정리

기존 코드에서 발견된 패턴 3종:

| 패턴 | 예시 | 사용 용도 |
|------|------|---------|
| `SUBSYSTEM_ROLE` | `FOOD_GRID_CORE`, `GAIA_SOT_CORE` | 신규 서브시스템 블록 (권장) |
| `HS_<phase>_<role>` | `HS_AGRO_HARVEST`, `HS_COLLISION_LOOP` | 핫픽스/이식 기원 블록 |
| `HS_FITTER_P<n>_<n2>_<role>` | `HS_FITTER_P4_1_INPUT_BINDINGS` | Fitter 패치 기원 블록 |

**신규 GAP 블록에는 `SUBSYSTEM_ROLE` 패턴을 권장.** `HS_` prefix는 핫픽스·이식 맥락에서만 사용.

### 4-6. 검증 체크리스트 (블록 추가 후)

```
[ ] 신규 START/END 마커 쌍이 대칭인지 확인
[ ] 기존 코드가 마커 주석 삽입 이외에 변경되지 않았는지 확인
[ ] 총 BUILDER_BLOCK 마커 수 재계산 (현재 214 → 추가 후 N)
[ ] 블록명 중복 없는지 grep으로 확인
[ ] 본 스냅샷 문서 라인번호 재갱신
[ ] BUILDER_BLOCK_SNAPSHOT_AUDIT.md에 변경 이력 기재
```

---

## 5. 부록: 이슈 목록

| 이슈 ID | 유형 | 블록명 | 위치 | 상태 |
|---------|------|--------|------|------|
| ISS-01 | 중복 블록명 | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` | 18082 & 18239 | 미해결 — 4-4절 참조 |
| ISS-02 | 대형 미분할 | `UI_NAV_ROUTING` | 9717–11469 (1752줄) | 중기 분할 권장 |
| ISS-03 | 대형 미분할 | `HS_COLLISION_LOOP` | 13715–14726 (1011줄) | 중기 분할 권장 |
| ISS-04 | 대형 미분할 | `AGENT_GRID_CORE` | 8094–9079 (985줄) | 중기 분할 권장 |
| ISS-05 | 대형 미분할 | `HS_PHASE1_HELPERS` | 16200–17093 (893줄) | 중기 분할 권장 |
| ISS-06 | GAP 누락 | *GAP-C* | 11714–12403 (690줄) | P0 — 즉시 처리 권장 |
| ISS-07 | GAP 누락 | *GAP-B* | 9479–9716 (238줄) | P1 |
| ISS-08 | GAP 누락 | *GAP-H* | 17923–18081 (159줄) | P1 |
| ISS-09 | GAP 누락 | *GAP-F* | 15186–15335 (150줄) | P2 |
| ISS-10 | GAP 누락 | *GAP-D* | 13600–13714 (115줄) | P2 |
| ISS-11 | GAP 누락 | *GAP-E* | 14727–14828 (102줄) | P3 |
| ISS-12 | GAP 누락 | *GAP-G* | 15790–15875 (86줄) | P3 |
| ISS-13 | GAP 누락 | *GAP-A* | 8037–8093 (57줄) | P4 |
| ISS-14 | GAP 누락 | *GAP-I* | 18397–18453 (57줄) | P4 |

---

*이 문서는 v27.11 코드 직접 추출 기반. 버전 변경 시 재동기화 필요.*
*라인번호 유효 범위: v27.11 (18,516줄) 기준.*
