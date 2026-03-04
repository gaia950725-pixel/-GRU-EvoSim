# BUILDER_BLOCK Snapshot — EvoSim index.html

**작성일자:** 2026-03-04
**대상 파일:** `index.html`
**기준 버전:** EvoSim 27.10 (terrain footing-load precise-alt)
**총 BUILDER_BLOCK 마커 수:** 214개 (START/END 쌍 포함)

---

## 목차

1. [전체 블록 목록 (라인 순서)](#1-전체-블록-목록)
2. [블록 계층 구조 (섹션별)](#2-블록-계층-구조)
3. [누락 구간 분석 및 블록명 추천](#3-누락-구간-분석-및-블록명-추천)

---

## 1. 전체 블록 목록

라인 번호는 `index.html` 기준이며 START 마커 기준으로 정렬됨.

| # | 블록명 | START 라인 | END 라인 | 규모(줄) | 계층 |
|---|--------|-----------|---------|---------|------|
| 1 | `STYLES` | 7 | 1713 | ~1706 | 최상위 |
| 2 | `CSS` | 9 | 1484 | ~1475 | STYLES 내 |
| 3 | `UI_DOM` | 1767 | 18510 | ~16743 | 최상위 |
| 4 | `HOME_OVERLAY` | 1788 | 1804 | ~16 | UI_DOM 내 |
| 5 | `UI_HOME` | 1789 | 1803 | ~14 | HOME_OVERLAY 내 |
| 6 | `CONFIG_OVERLAY` | 1806 | 2182 | ~376 | UI_DOM 내 |
| 7 | `UI_CONFIG` | 1807 | 2180 | ~373 | CONFIG_OVERLAY 내 |
| 8 | `UI_INGAME` | 2185 | 2361 | ~176 | UI_DOM 내 |
| 9 | `ARCHITECTURE_OVERVIEW` | 2428 | 2466 | ~38 | 독립(주석블록) |
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
| 58 | `UI_NAV_ROUTING` | 9717 | 11464 | ~1747 | 최상위 JS (대형) |
| 59 | `UMC_LEGACY_WRAPPERS_REMOVED` | 9898 | 9904 | ~6 | UI_NAV_ROUTING 내 |
| 60 | `HS_19_CLAMOR_RESET_ON_START_INSERT` | 10444 | 10466 | ~22 | UI_NAV_ROUTING 내 |
| 61 | `HS_A3_POP_RESET_REPLACE` | 10470 | 10473 | ~3 | UI_NAV_ROUTING 내 |
| 62 | `HS_FITTER_P1_1_PATH2D_CACHE` | 10805 | 10813 | ~8 | UI_NAV_ROUTING 내 |
| 63 | `HS_STATS_FARM_DROP_INIT` | 10872 | 10874 | ~2 | UI_NAV_ROUTING 내 |
| 64 | `HS_AGGVIZ_UPDATE_MOVED` | 11079 | 11099 | ~20 | UI_NAV_ROUTING 내 |
| 65 | `HS_AGENT_UPDATE` | 11466 | 11564 | ~98 | 최상위 JS |
| 66 | `HS_FITTER_P1_1_AGENT_TRI_REPLACE` | 11681 | 11708 | ~27 | 최상위 JS |
| 67 | `ATLAS_P1_OVERVIEW_VARS` | 12399 | 12404 | ~5 | 최상위 JS |
| 68 | `HS_A3_POP_RING_REPLACE` | 12487 | 12571 | ~84 | 최상위 JS |
| 69 | `HS_FITTER_P1_2_POP_SAMPLE` | 12565 | 12570 | ~5 | HS_A3_POP_RING_REPLACE 내 |
| 70 | `ATLAS_P1_STATICMAP_CLIP` | 12603 | 13165 | ~562 | 최상위 JS |
| 71 | `ATLAS_P1_OVERVIEW_CALL` | 13188 | 13191 | ~3 | 최상위 JS |
| 72 | `ATLAS_P1_OVERVIEW_FN` | 13194 | 13216 | ~22 | 최상위 JS |
| 73 | `SPAWN_AND_LOG_CORE` | 13218 | 13593 | ~375 | 최상위 JS |
| 74 | `HS_FITTER_P1_1_RENDERLOGS_GUARD` | 13572 | 13577 | ~5 | SPAWN_AND_LOG_CORE 내 |
| 75 | `HS_COLLISION_LOOP` | 13709 | 14720 | ~1011 | 최상위 JS (대형) |
| 76 | `HS_APPLYEAT_FARM_DROP_STATS` | 14451 | 14456 | ~5 | HS_COLLISION_LOOP 내 |
| 77 | `HS_NAT_AREA_SCALE` | 14487 | 14490 | ~3 | HS_COLLISION_LOOP 내 |
| 78 | `HS_FITTER_P4_1_INPUT_BINDINGS` | 14823 | 15124 | ~301 | 최상위 JS |
| 79 | `HS_HOTKEYS` | 14835 | 14978 | ~143 | HS_FITTER_P4_1_INPUT_BINDINGS 내 |
| 80 | `HS_A3_KEYP_REPLACE` | 14959 | 14976 | ~17 | HS_HOTKEYS 내 |
| 81 | `ERA_BTN_SURNAME_AUTO_WIRE` | 15127 | 15179 | ~52 | 최상위 JS |
| 82 | `HS_FARM_OBS_MINI_BLOCKVIEW` | 15330 | 15480 | ~150 | 최상위 JS |
| 83 | `HS_BRAIN_THROTTLE_HELPER` | 15503 | 15540 | ~37 | 최상위 JS |
| 84 | `UI_INSPECTOR_PANEL_UPDATE` | 15542 | 15702 | ~160 | 최상위 JS |
| 85 | `HS_INSPECTOR_SUBST_FARM_DROP_ROW` | 15634 | 15636 | ~2 | UI_INSPECTOR_PANEL_UPDATE 내 |
| 86 | `HS_FITTER_P4_1_UITICK_REGION` | 15704 | 15729 | ~25 | 최상위 JS |
| 87 | `HS_UITICK_WRAPPERS` | 15706 | 15728 | ~22 | HS_FITTER_P4_1_UITICK_REGION 내 |
| 88 | `HS_FITTER_P1_4_UITICK500_ROUTER` | 15707 | 15723 | ~16 | HS_UITICK_WRAPPERS 내 |
| 89 | `HS_FITTER_P1_1_PANELVISIBLE` | 15731 | 15741 | ~10 | 최상위 JS |
| 90 | `HS_FITTER_P1_2_HISTORY_UI` | 15743 | 15783 | ~40 | 최상위 JS |
| 91 | `HS_FITTER_P1_3_HISTORY_DRAW_CONTROL` | 15759 | 15778 | ~19 | HS_FITTER_P1_2_HISTORY_UI 내 |
| 92 | `HS_A3_DEV_PERF_API` | 15870 | 15915 | ~45 | 최상위 JS |
| 93 | `HS_FITTER_P4_1_RENDER_LOOP` | 15935 | 16189 | ~254 | 최상위 JS |
| 94 | `HS_PHASE1_HELPERS` | 16194 | 17087 | ~893 | 최상위 JS (대형) |
| 95 | `HS_FITTER_P4_1_SIM_LOOP` | 16195 | 16884 | ~689 | HS_PHASE1_HELPERS 내 |
| 96 | `HS_19_SIMLOOP_DECAY_INSERT` | 16370 | 16376 | ~6 | HS_FITTER_P4_1_SIM_LOOP 내 |
| 97 | `HS_AGRO_SELECTED_TILE_LAZY_TOUCH` | 16378 | 16392 | ~14 | HS_FITTER_P4_1_SIM_LOOP 내 |
| 98 | `AUTOCLAN_ANIMATE_HOOK` | 16393 | 16401 | ~8 | HS_FITTER_P4_1_SIM_LOOP 내 |
| 99 | `HS_RAF_SINGLETON_21_1` | 17089 | 17120 | ~31 | 최상위 JS |
| 100 | `HS_DISPOSE_WORLD_21_1` | 17122 | 17185 | ~63 | 최상위 JS |
| 101 | `HS_MONITOR_CORE` | 17216 | 17823 | ~607 | 최상위 JS |
| 102 | `DEV_CONSOLE_HELPERS` | 17468 | 17651 | ~183 | HS_MONITOR_CORE 내 |
| 103 | `HS_FITTER_P3_1_CORE_API` | 17885 | 17916 | ~31 | 최상위 JS |
| 104 | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` *(1st)* | 18076 | 18081 | ~5 | UI_DRAWERS IIFE 내 |
| 105 | `UI_DRAWER_TOGGLES_CORE` | 18208 | 18390 | ~182 | 최상위 JS |
| 106 | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` *(2nd)* | 18233 | 18238 | ~5 | UI_DRAWER_TOGGLES_CORE 내 |
| 107 | `HS_FITTER_P1_3_TREND_TAB_DIRTY` | 18448 | 18457 | ~9 | 최상위 JS |

> **주의:** `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY`는 동일 블록명이 두 위치(18076, 18233)에 중복 존재함.

---

## 2. 블록 계층 구조

```
index.html (18,510+ 줄)
├── STYLES (7–1713)
│   └── CSS (9–1484)
├── UI_DOM (1767–18510)
│   ├── HOME_OVERLAY (1788–1804)
│   │   └── UI_HOME (1789–1803)
│   ├── CONFIG_OVERLAY (1806–2182)
│   │   └── UI_CONFIG (1807–2180)
│   └── UI_INGAME (2185–2361)
│
│   [내부 <script> 블록 — JS 구간]
│
├── ARCHITECTURE_OVERVIEW (2428–2466)          ← 설계 문서 주석
├── JS_TOP (2468–2472)                          ← import/전역 선언
├── JS_BOOTSTRAP_CORE (2473–2863)              ← 초기화 코어
├── SURNAME_AUTO_CORE (2864–3284)
│   └── SURNAME_AUTO_SANITIZE_CLAN (3168–3183)
├── WORLDGEN_UI_INIT (3288–3748)               ← 월드 생성 UI 초기화
│   ├── CONFIG_TABS (3462–3578)
│   ├── HS_FITTER_P1_3_GRAPH_FLAGS (3697–3700)
│   ├── HS_FITTER_P1_3A_HISTORY_ADAPTER (3701–3714)
│   └── HS_FITTER_P1_3_PANEL_EXPAND_DIRTY (3737–3743)
├── CFG_DEFAULTS (3750–3998)                   ← 시뮬레이션 설정값
├── NN_IO_ENUMS (3999–4108)                    ← NN 입출력 슬롯 열거
├── VISION_CACHE_DERIVED (4109–4125)           ← 시야 파생 캐시
├── HS_OCCLUSION_21RAY_GLOBALS (4127–4241)    ← 시야 차폐 21레이 전역
├── AGRO_GLOBALS (4242–4253)                   ← 농업 전역변수
├── HS_AGRO_BLOCKVIEW_UTILS (4262–4448)
├── UI_INPUT_WORLD_UTILS (4452–4483)
├── HS_AGRO_TOUCH_REPIDX (4487–4489)
├── HS_AGRO_PEEK_TILE (4520–4541)
├── HS_AGRO_TOP4_REPIDX (4548–4550)
├── HS_AGRO_HARVEST (4608–4729)
│   ├── HS_AGRO_HARVEST_REPIDX (4611–4613)
│   └── HS_AGRO_HARVEST_FARMDROP (4658–4712)
├── AGRO_FARM_WORK_TICK (4732–4834)
│   └── HS_AGRO_WORK_REPIDX (4740–4743)
├── CORE_MATH_ENERGY_UTILS (4836–5031)         ← 에너지/수학 유틸
├── AGRO_INIT_CORE (5032–5322)
│   ├── HS_INITAGRO_FARMBUILD_BLOCKS (5094–5243)
│   └── HS_INITAGRO_FARMSUIT_BLOCKS (5269–5320)
├── GAIA_30PX_LOOKUP_APIS (5327–5479)          ← GAIA 30px 조회 API
├── TIME_NARRATIVE_GLOBALS (5481–5566)          ← 시간/서사 전역
├── MAP_PRESETS (5568–5654)                     ← 맵 프리셋
├── GAIA_SOT_CORE (5659–6070)                  ← GAIA 단일진실원천 코어
├── GAIA_MOUNTAINS (6072–6543)
│   └── GAIA_MTN_POLICY (6081–6092)
├── GAIA_ENV30 (6545–7123)                     ← 기후/환경 30px 시스템
├── SIM_GLOBALS_POST_GAIA_ENV30 (7124–7710)    ← 시뮬레이션 전역 (GAIA 이후)
├── HS_19_CLAMOR_CORE_ADD (7712–8036)          ← 소음/음성 이벤트 코어
│
│   ⚠ [GAP-A] 8036–8094 (~58줄) — 미래 블록 없음
│
├── AGENT_GRID_CORE (8094–9079)                ← 에이전트 그리드 코어 (985줄)
│   ├── HS_18_7_GLOBALS (8098–8105)
│   └── HS_18_7_BUILD_FUNC_REPLACE (8122–8197)
├── GRID_QUERY_API (9080–9118)
│   └── HS_QUERY_API (9084–9116)
├── HS_18_7_ADD_AGENT_REPLACE (9128–9151)
├── GRIDDBG_HELPERS (9152–9224)                ← 그리드 디버그 헬퍼
├── HERB_GRID_CORE (9225–9348)
│   ├── HS_A2_HERBGRID_GLOBALS (9246–9253)
│   └── HS_A2_BUILD_HERBGRID_REPLACE (9274–9331)
├── PRED_GRID_CORE (9349–9478)                 ← 포식자 그리드 코어
│
│   ⚠ [GAP-B] 9478–9717 (~239줄) — 미래 블록 없음
│
├── UI_NAV_ROUTING (9717–11464)                ← UI 라우팅 코어 (1747줄)
│   ├── UMC_LEGACY_WRAPPERS_REMOVED (9898–9904)
│   ├── HS_19_CLAMOR_RESET_ON_START_INSERT (10444–10466)
│   ├── HS_A3_POP_RESET_REPLACE (10470–10473)
│   ├── HS_FITTER_P1_1_PATH2D_CACHE (10805–10813)
│   ├── HS_STATS_FARM_DROP_INIT (10872–10874)
│   └── HS_AGGVIZ_UPDATE_MOVED (11079–11099)
├── HS_AGENT_UPDATE (11466–11564)              ← 에이전트 업데이트
├── HS_FITTER_P1_1_AGENT_TRI_REPLACE (11681–11708)
│
│   ⚠ [GAP-C] 11708–12399 (~691줄) — 미래 블록 없음 ★ 최대
│
├── ATLAS_P1_OVERVIEW_VARS (12399–12404)
├── HS_A3_POP_RING_REPLACE (12487–12571)
│   └── HS_FITTER_P1_2_POP_SAMPLE (12565–12570)
├── ATLAS_P1_STATICMAP_CLIP (12603–13165)      ← 정적 지도 클립 렌더링
├── ATLAS_P1_OVERVIEW_CALL (13188–13191)
├── ATLAS_P1_OVERVIEW_FN (13194–13216)
├── SPAWN_AND_LOG_CORE (13218–13593)           ← 스폰/로그 코어
│   └── HS_FITTER_P1_1_RENDERLOGS_GUARD (13572–13577)
│
│   ⚠ [GAP-D] 13593–13709 (~116줄) — 미래 블록 없음
│
├── HS_COLLISION_LOOP (13709–14720)            ← 충돌/물리 루프 (1011줄)
│   ├── HS_APPLYEAT_FARM_DROP_STATS (14451–14456)
│   └── HS_NAT_AREA_SCALE (14487–14490)
│
│   ⚠ [GAP-E] 14720–14823 (~103줄) — 미래 블록 없음
│
├── HS_FITTER_P4_1_INPUT_BINDINGS (14823–15124)← 입력 바인딩
│   └── HS_HOTKEYS (14835–14978)
│       └── HS_A3_KEYP_REPLACE (14959–14976)
├── ERA_BTN_SURNAME_AUTO_WIRE (15127–15179)
│
│   ⚠ [GAP-F] 15179–15330 (~151줄) — 미래 블록 없음
│
├── HS_FARM_OBS_MINI_BLOCKVIEW (15330–15480)
├── HS_BRAIN_THROTTLE_HELPER (15503–15540)
├── UI_INSPECTOR_PANEL_UPDATE (15542–15702)    ← 인스펙터 패널 업데이트
│   └── HS_INSPECTOR_SUBST_FARM_DROP_ROW (15634–15636)
├── HS_FITTER_P4_1_UITICK_REGION (15704–15729)
│   └── HS_UITICK_WRAPPERS (15706–15728)
│       └── HS_FITTER_P1_4_UITICK500_ROUTER (15707–15723)
├── HS_FITTER_P1_1_PANELVISIBLE (15731–15741)
├── HS_FITTER_P1_2_HISTORY_UI (15743–15783)
│   └── HS_FITTER_P1_3_HISTORY_DRAW_CONTROL (15759–15778)
│
│   ⚠ [GAP-G] 15783–15870 (~87줄) — 미래 블록 없음
│
├── HS_A3_DEV_PERF_API (15870–15915)
├── HS_FITTER_P4_1_RENDER_LOOP (15935–16189)   ← 렌더 루프
├── HS_PHASE1_HELPERS (16194–17087)            ← Phase1 헬퍼 (893줄)
│   └── HS_FITTER_P4_1_SIM_LOOP (16195–16884) ← 시뮬레이션 메인 루프
│       ├── HS_19_SIMLOOP_DECAY_INSERT (16370–16376)
│       ├── HS_AGRO_SELECTED_TILE_LAZY_TOUCH (16378–16392)
│       └── AUTOCLAN_ANIMATE_HOOK (16393–16401)
├── HS_RAF_SINGLETON_21_1 (17089–17120)        ← RAF 싱글턴
├── HS_DISPOSE_WORLD_21_1 (17122–17185)        ← 월드 해제
├── HS_MONITOR_CORE (17216–17823)              ← 모니터링 코어
│   └── DEV_CONSOLE_HELPERS (17468–17651)
├── HS_FITTER_P3_1_CORE_API (17885–17916)
│
│   ⚠ [GAP-H] 17916–18076 (~160줄) — 미래 블록 없음
│
├── [UI_DRAWERS_STEP1 IIFE — 블록명 없음]
│   └── HS_FITTER_P1_3_TOGGLERIGHT_DIRTY (18076–18081) ← IIFE 내 조각
├── UI_DRAWER_TOGGLES_CORE (18208–18390)
│   └── HS_FITTER_P1_3_TOGGLERIGHT_DIRTY (18233–18238) ← ⚠ 중복명
│
│   ⚠ [GAP-I] 18390–18448 (~58줄) — 미래 블록 없음
│
└── HS_FITTER_P1_3_TREND_TAB_DIRTY (18448–18457)
```

---

## 3. 누락 구간 분석 및 블록명 추천

총 **9개 GAP 구간** 발견. 규모 순으로 정렬.

---

### GAP-C ★ (최우선) — 라인 11708–12399 (~691줄)

**현황:** `HS_FITTER_P1_1_AGENT_TRI_REPLACE_END`와 `ATLAS_P1_OVERVIEW_VARS_START` 사이.
**포함 코드:**
- `class Predator { ... }` — 포식자 클래스 전체 정의
- `function _ss_applySolitaryActions(a, simulatedTicks)` — 개별 행동 시뮬레이션
- `class Herbivore { ... }` — 초식동물 클래스 전체 정의

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `PREDATOR_CLASS_CORE_START/END` | Predator 클래스 전체 | 포식자 개체 클래스 |
| `SOLITARY_ACTION_HELPERS_START/END` | `_ss_applySolitaryActions` 함수 | 개체 고독행동 헬퍼 |
| `HERBIVORE_CLASS_CORE_START/END` | Herbivore 클래스 전체 | 초식동물 개체 클래스 |

또는 단일 상위 블록으로:

| 블록명 | 범위 |
|--------|------|
| `ENTITY_CLASSES_PRED_HERB_START/END` | 11709–12398 전체 묶음 |

---

### GAP-B — 라인 9478–9717 (~239줄)

**현황:** `PRED_GRID_CORE_END`와 `UI_NAV_ROUTING_START` 사이.
**포함 코드:**
- `localStorage` 복원 (predThreat, leaderMode)
- 속도/식량/일시정지 슬라이더 이벤트 바인딩
- Step2-A: Control Dock 초기화 IIFE

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `UI_STARTUP_CONTROLS_INIT_START/END` | 9478–9717 전체 | 시작 시 UI 컨트롤 복원/초기화 |
| `LOCALSTORAGE_RESTORE_INIT_START/END` | localStorage 복원 부분 | 설정값 복원 서브블록 |

---

### GAP-H — 라인 17916–18076 (~160줄)

**현황:** `HS_FITTER_P3_1_CORE_API_END`와 `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_START` 사이.
**포함 코드:**
- `(function __initUIDrawersStep1(){ ... })()` — UI 드로어 Step1 IIFE 전체
  - `isMobile()`, `_clampMode()`, `_readMode()`, `loadSavedState()`, `saveState()`, `apply()`, `toggleRight()` 등

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `UI_DRAWERS_STEP1_INIT_START/END` | 17917–18075 | UI 드로어 Step1 초기화 IIFE |

---

### GAP-F — 라인 15179–15330 (~151줄)

**현황:** `ERA_BTN_SURNAME_AUTO_WIRE_END`와 `HS_FARM_OBS_MINI_BLOCKVIEW_START` 사이.
**포함 코드:**
- `function updateSystemPanels()` — 씨족/전쟁 등 시스템 패널 업데이트 함수

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `UI_SYSTEM_PANELS_UPDATE_START/END` | 15180–15329 | 시스템 패널(씨족/전쟁) 업데이트 |

---

### GAP-D — 라인 13593–13709 (~116줄)

**현황:** `SPAWN_AND_LOG_CORE_END`와 `HS_COLLISION_LOOP_START` 사이.
**포함 코드:**
- `function _ensureFoodGridDims()`
- `class FoodGrid1D { ... }` — 1D 먹이 그리드 클래스
- `_foodCellId()`, `_buildFoodGrid()`, `_forNeighborFoodCells()`, `_forFoodCellsInRadius()`, `_inFront180_fromAngle()` 등

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `FOOD_GRID_CORE_START/END` | 13594–13708 | 먹이 그리드 클래스 및 유틸 |

---

### GAP-E — 라인 14720–14823 (~103줄)

**현황:** `HS_COLLISION_LOOP_END`와 `HS_FITTER_P4_1_INPUT_BINDINGS_START` 사이.
**포함 코드:**
- `function drawBrain(ctx, brain)` — 신경망 시각화 캔버스 렌더링 함수

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `BRAIN_CANVAS_DRAW_START/END` | 14721–14822 | brainCanvas 신경망 렌더링 |

---

### GAP-G — 라인 15783–15870 (~87줄)

**현황:** `HS_FITTER_P1_2_HISTORY_UI_END`와 `HS_A3_DEV_PERF_API_START` 사이.
**포함 코드:**
- `function _ensureFoodGridCurrent()` — 먹이 그리드 최신화 보장
- Phase0 Policy (Grid/Death/Cache Unification) 관련 코드
- `function _ssEnsureTickExt(n)` — 솔리터리 틱 확장

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `FOOD_PHASE0_SIM_UTILS_START/END` | 15784–15869 | 먹이 그리드 현행화 + Phase0 유틸 |

---

### GAP-A — 라인 8036–8094 (~58줄)

**현황:** `HS_19_CLAMOR_CORE_ADD_END`와 `AGENT_GRID_CORE_START` 사이.
**포함 코드:**
- `const TS_CL_VOICE_EVENTS = []` — 음성 이벤트 큐 전역
- `function TS_evalShoutEveryTick(tick)` — 외향성 기반 외침 평가
- `let TS_solitarySkip` / `TS_solitarySkipTouched` — 솔리터리 스킵 배열 전역

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `TS_SOCIAL_VOICE_GLOBALS_START/END` | 8037–8093 | 음성/사회행동 전역 변수 및 평가함수 |

---

### GAP-I — 라인 18390–18448 (~58줄)

**현황:** `UI_DRAWER_TOGGLES_CORE_END`와 `HS_FITTER_P1_3_TREND_TAB_DIRTY_START` 사이.
**포함 코드:**
- `/* === PNREV_P2: chip tabs init (Rev31 only) === */` IIFE — 트렌드/생물통계 칩탭 초기화

**추천 블록명:**

| 블록명 | 범위 | 설명 |
|--------|------|------|
| `UI_CHIP_TABS_PNREV_INIT_START/END` | 18391–18447 | Rev31 칩탭(트렌드/바이오) 초기화 |

---

## 부록: 중복/이상 블록 목록

| 이슈 | 블록명 | 위치 |
|------|--------|------|
| **중복 블록명** | `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` | 18076 & 18233 — 동일 이름 두 곳 존재 |
| **매우 큰 미분할 블록** | `UI_NAV_ROUTING` | 9717–11464 (1747줄) — 내부 세분화 권장 |
| **매우 큰 미분할 블록** | `HS_COLLISION_LOOP` | 13709–14720 (1011줄) — 내부 세분화 권장 |
| **매우 큰 미분할 블록** | `AGENT_GRID_CORE` | 8094–9079 (985줄) — 내부 세분화 권장 |
| **매우 큰 미분할 블록** | `HS_PHASE1_HELPERS` | 16194–17087 (893줄) — 내부 세분화 권장 |

---

*이 문서는 자동 전수조사 기반으로 작성됨. 블록 추가/변경 시 재조사 권장.*
