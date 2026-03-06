# PHASE1_INSPECT_RESULTS — v27.20_fix1

작성일: 2026-03-05  
대상: `index.html` (비파괴 점검, 코드/마커 변경 없음)

## P0-A 라인번호 재스캔

- 스캔 결과: 고유 블록 118, START 118, END 118, 이름별 START/END 불일치 없음.
- 산출물: `block_scan_v2720fix1.json` 생성.

### SNAPSHOT 대비 ±5줄 이상 시프트 블록
총 41개 블록에서 시프트 확인.

- `UI_NAV_ROUTING` +9
- `ATLAS_P1_OVERVIEW_VARS` +14
- `HS_A3_POP_RING_REPLACE` +14
- `HS_FITTER_P1_2_POP_SAMPLE` +14
- `ATLAS_P1_STATICMAP_CLIP` +14
- `ATLAS_P1_OVERVIEW_CALL` +14
- `ATLAS_P1_OVERVIEW_FN` +14
- `SPAWN_AND_LOG_CORE` +14
- `HS_FITTER_P1_1_RENDERLOGS_GUARD` +14
- `HS_COLLISION_LOOP` +14
- `HS_APPLYEAT_FARM_DROP_STATS` +14
- `HS_NAT_AREA_SCALE` +14
- `HS_FITTER_P4_1_INPUT_BINDINGS` +14
- `HS_HOTKEYS` +14
- `HS_A3_KEYP_REPLACE` +14
- `ERA_BTN_SURNAME_AUTO_WIRE` +14
- `HS_FARM_OBS_MINI_BLOCKVIEW` +14
- `HS_BRAIN_THROTTLE_HELPER` +14
- `UI_INSPECTOR_PANEL_UPDATE` +14
- `HS_INSPECTOR_SUBST_FARM_DROP_ROW` +14
- `HS_FITTER_P4_1_UITICK_REGION` +14
- `HS_UITICK_WRAPPERS` +14
- `HS_FITTER_P1_4_UITICK500_ROUTER` +14
- `HS_FITTER_P1_1_PANELVISIBLE` +14
- `HS_FITTER_P1_2_HISTORY_UI` +14
- `HS_FITTER_P1_3_HISTORY_DRAW_CONTROL` +14
- `HS_A3_DEV_PERF_API` +14
- `HS_FITTER_P4_1_RENDER_LOOP` +14
- `HS_PHASE1_HELPERS` +14
- `HS_FITTER_P4_1_SIM_LOOP` +14
- `HS_19_SIMLOOP_DECAY_INSERT` +14
- `HS_AGRO_SELECTED_TILE_LAZY_TOUCH` +14
- `AUTOCLAN_ANIMATE_HOOK` +14
- `HS_RAF_SINGLETON_21_1` +14
- `HS_DISPOSE_WORLD_21_1` +14
- `HS_MONITOR_CORE` +14
- `DEV_CONSOLE_HELPERS` +14
- `HS_FITTER_P3_1_CORE_API` +14
- `UI_DRAWER_TOGGLES_CORE` +16
- `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY` (2nd 위치 기준 비교 시) +173
- `HS_FITTER_P1_3_TREND_TAB_DIRTY` +16

---

## S1 — STYLES ~ UI_INGAME

| 블록명 | 라인(현재) | 귀속 모듈 | 판정 | 권고 조치 | 비고 |
|---|---:|---|---|---|---|
| STYLES | 7–1711 | M01 | 정상 | 유지 | 이름/내용 일치 |
| CSS | 9–1482 | M01 | 정상 | 유지 | STYLES 내부 |
| UI_DOM | 1765–18532 | M13 | 정상 | 유지 | HTML 루트 컨테이너 |
| HOME_OVERLAY | 1786–1802 | M13 | 정상 | 유지 | UI_DOM 내부 |
| UI_HOME | 1787–1801 | M13 | 정상 | 유지 | HOME_OVERLAY 내부 |
| CONFIG_OVERLAY | 1804–2180 | M13 | 정상 | 유지 | DOM 오버레이 정의 |
| UI_CONFIG | 1805–2178 | M13/M04 경계 | 정상 | 유지 | DOM 구조는 M13, 로직은 M04 분리 가능 |
| UI_INGAME | 2183–2359 | M13 | 정상 | 유지 | 게임 HUD DOM |

---

## S2 — ARCHITECTURE_OVERVIEW ~ AGRO_INIT_CORE

| 블록명 | 라인(현재) | 귀속 모듈 | 판정 | 권고 조치 | 비고 |
|---|---:|---|---|---|---|
| ARCHITECTURE_OVERVIEW | 2426–2464 | 공통 설계주석 | 정상 | 유지 | 문서성 블록 |
| JS_TOP | 2466–2470 | M02 | 정상 | 유지 | bootstrap 상단 |
| JS_BOOTSTRAP_CORE | 2471–2861 | M02 | 정상 | 유지 | bootstrap 코어 |
| SURNAME_AUTO_CORE | 2862–3282 | M09 | 정상 | 유지 | social 코어 |
| SURNAME_AUTO_SANITIZE_CLAN | 3166–3181 | M09 | 정상 | 유지 | 내부 sanitize |
| WORLDGEN_UI_INIT | 3286–3746 | M04 | C2 | `UI_CFG_SCREEN_INIT` 개명 + 하위 계층화 | config/worldgen 혼재 |
| CONFIG_TABS | 3460–3576 | M04 | C1 | `UI_CFG_TABS` 개명 | UI config 탭 |
| HS_FITTER_P1_3_GRAPH_FLAGS | 3695–3698 | M04 | C4 | `UI_CFG_GRAPH_FLAGS` 개명 | HS 유물 |
| HS_FITTER_P1_3A_HISTORY_ADAPTER | 3699–3712 | M04 | C4 | `UI_CFG_HISTORY_ADAPTER` 개명 | HS 유물 |
| HS_FITTER_P1_3_PANEL_EXPAND_DIRTY | 3735–3741 | M04 | C4 | `UI_CFG_PANEL_EXPAND` 개명 | HS 유물 |
| CFG_DEFAULTS | 3748–3996 | M03 | 정상 | 유지 | pure defaults |
| NN_IO_ENUMS | 3997–4106 | M03 | 정상 | 유지 | CFG_DEFAULTS 인접 적절 |
| VISION_CACHE_DERIVED | 4107–4123 | M06 | 정상 | 유지 | agent vision 파생값 |
| HS_OCCLUSION_21RAY_GLOBALS | 4125–4239 | M06 | C4 | `VISION_OCCLUSION_GLOBALS` 개명 | HS 유물 |
| AGRO_GLOBALS | 4240–4251 | M08 | 정상 | 유지 | food globals |
| HS_AGRO_BLOCKVIEW_UTILS | 4260–4446 | M08 | C4 | `AGRO_BLOCKVIEW_UTILS` 개명 | HS 유물 |
| UI_INPUT_WORLD_UTILS | 4450–4481 | M08/M12 경계 | C2 | `UI_INPUT_COORD_UTILS` + `AGRO_HOVER_INPUT` 분리 | ⚠️ 아래 상세 |
| HS_AGRO_TOUCH_REPIDX | 4485–4487 | M08 | C4 | `AGRO_TOUCH_REPIDX` 개명 | HS 유물 |
| HS_AGRO_PEEK_TILE | 4518–4539 | M08 | C4 | `AGRO_PEEK_TILE` 개명 | HS 유물 |
| HS_AGRO_TOP4_REPIDX | 4546–4548 | M08 | C4 | `AGRO_TOP4_REPIDX` 개명 | HS 유물 |
| HS_AGRO_HARVEST | 4606–4727 | M08 | C4 | `AGRO_HARVEST` 개명 | HS 유물 |
| HS_AGRO_HARVEST_REPIDX | 4609–4611 | M08 | C4 | `AGRO_HARVEST_REPIDX` 개명 | 내부 |
| HS_AGRO_HARVEST_FARMDROP | 4656–4710 | M08 | C4 | `AGRO_HARVEST_FARMDROP` 개명 | 내부 |
| AGRO_FARM_WORK_TICK | 4730–4832 | M08 | 정상 | 유지 | food tick |
| HS_AGRO_WORK_REPIDX | 4738–4741 | M08 | C4 | `AGRO_WORK_REPIDX` 개명 | 내부 |
| CORE_MATH_ENERGY_UTILS | 4834–5029 | M08 | 정상 | 유지 | 공용 유틸(현 food 문맥) |
| AGRO_INIT_CORE | 5030–5320 | M08 | 정상 | 유지 | food init core |
| HS_INITAGRO_FARMBUILD_BLOCKS | 5092–5241 | M08 | C4 | `AGRO_FARM_BUILD` 개명 | 내부 |
| HS_INITAGRO_FARMSUIT_BLOCKS | 5267–5318 | M08 | C4 | `AGRO_FARM_SUIT` 개명 | 내부 |

### ⚠️ UI_INPUT_WORLD_UTILS 내용 요약/판단
- `clientToWorld()`는 캔버스 좌표 변환(UI 입력 공통)이며, `updateAgroHoverFromClient()`는 `AGRO.farmMask`를 조회해 농지 hover 상태를 갱신함.
- 단일 블록 내 UI 입력 변환과 food 하이라이트 로직이 함께 존재하므로 C2로 판정. 귀속은 1차 M12(UI) + 2차 M08(food)로 분리 권장.

---

## S3 — GAIA_30PX ~ PRED_GRID_CORE

| 블록명 | 라인(현재) | 귀속 모듈 | 판정 | 권고 조치 | 비고 |
|---|---:|---|---|---|---|
| GAIA_30PX_LOOKUP_APIS | 5325–5477 | M05 | 정상 | 유지 | |
| TIME_NARRATIVE_GLOBALS | 5479–5564 | M05 | 정상 | 유지 | |
| MAP_PRESETS | 5566–5652 | M05 | 정상 | 유지 | |
| GAIA_SOT_CORE | 5657–6068 | M05 | 정상 | 유지 | |
| GAIA_MOUNTAINS | 6070–6541 | M05 | 정상 | 유지 | |
| GAIA_MTN_POLICY | 6079–6090 | M05 | 정상 | 유지 | |
| GAIA_ENV30 | 6543–7121 | M05 | 정상 | 유지 | |
| SIM_GLOBALS_POST_GAIA_ENV30 | 7122–7708 | M10 | C2 | sim 전역/도메인별 하위 블록 분해 | ⚠️ 아래 상세 |
| HS_19_CLAMOR_CORE_ADD | 7710–8034 | M09 | C4 | `SOCIAL_CLAMOR_CORE` 개명 | |
| AGENT_GRID_CORE | 8092–9077 | M06 | C2 | 3분할 권고 유지 | ISS-04 |
| HS_18_7_GLOBALS | 8096–8103 | M06 | C4 | `AGENT_GRID_GLOBALS` 개명 | 내부 |
| HS_18_7_BUILD_FUNC_REPLACE | 8120–8195 | M06 | C4 | `AGENT_GRID_BUILD` 개명 | 내부 |
| GRID_QUERY_API | 9078–9116 | M06 | 정상 | 유지 | |
| HS_QUERY_API | 9082–9114 | M06 | C4 | `AGENT_QUERY_CORE` 개명 | 내부 |
| HS_18_7_ADD_AGENT_REPLACE | 9126–9149 | M06 | C4 | `AGENT_GRID_ADD` 개명 | |
| GRIDDBG_HELPERS | 9150–9222 | M06 | 정상 | 유지 | |
| HERB_GRID_CORE | 9223–9346 | M07 | 정상 | 유지 | |
| HS_A2_HERBGRID_GLOBALS | 9244–9251 | M07 | C4 | `HERB_GRID_GLOBALS` 개명 | 내부 |
| HS_A2_BUILD_HERBGRID_REPLACE | 9272–9329 | M07 | C4 | `HERB_GRID_BUILD` 개명 | 내부 |
| PRED_GRID_CORE | 9347–9476 | M07 | 정상 | 유지 | |

### ⚠️ SIM_GLOBALS_POST_GAIA_ENV30 상세 분해 (100줄 단위)
- 7122–7221: 지형/이동 상수, predator/herb profile, 공격 쿨다운, 공헌도 분배 함수군.
- 7222–7321: 공격/번식 게이트 함수(`canAgentAttack`, `canAgentsRepro`), direwolf spawn/respawn 큐 함수.
- 7322–7421: pack hardening tick, `isLandmass`/`isPassable`/`isLand` 공간 판정.
- 7422–7521: `getRandomLandPoint`, 기후 가중치 food anchor 정책 함수군.
- 7522–7621: 최근 anchor 버퍼, 편향 랜덤 spawn, food eat anchor 정책.
- 7622–7708: 시뮬 전역 상태(`ERA`, `simulatedTicks`, camera 등), NAT area 스케일, agent grid 차원 준비.

분류:
- 전역변수 선언: 다수 (`ERA`, `GENERATION`, `paused`, cache/queue 등).
- 함수 선언: 다수 (공격, 번식, 패킹, 지형판정, food spawn, grid dims).
- 클래스 선언: 없음.
- 이벤트 바인딩: 없음.

귀속 판단:
- 사회(social) 전용보다 시뮬레이션 런타임 상태/정책이 지배적이므로 M10(sim.js) 귀속이 타당.
- 다만 food/predator/공간유틸이 혼재되어 C2(분할) 권고.

AGENT_GRID_CORE 분할 권고 유효성:
- 현 985줄 규모 유지 시 책임 경계 약함. `AGENT_CLASS_CORE / AGENT_NN_SENSOR / AGENT_GENETICS_CORE` 3분할 권고 유효.

---

## S4 — UI_STARTUP_CONTROLS_INIT ~ HS_BRAIN_THROTTLE_HELPER

| 블록명 | 라인(현재) | 귀속 모듈 | 판정 | 권고 조치 | 비고 |
|---|---:|---|---|---|---|
| UI_STARTUP_CONTROLS_INIT | 9477–9722 | M13 | 정상 | 유지 | 27.20 신설 |
| CONTROL_DOCK_STEP2A_INIT | 9536–9666 | M13 | 정상 | 유지 | 내부 |
| UI_NAV_ROUTING | 9726–11471 | M12 | C2 | 3분할 권고 유지 | ISS-02 |
| HS_19_CLAMOR_RESET_ON_START_INSERT | 10446–10468 | M12 | C4 | `UI_NAV_CLAMOR_RESET` 개명 | 내부 |
| HS_A3_POP_RESET_REPLACE | 10472–10475 | M12 | C4 | `UI_NAV_POP_RESET` 개명 | 내부 |
| HS_FITTER_P1_1_PATH2D_CACHE | 10812–10820 | M12 | C4 | `UI_NAV_PATH2D_CACHE` 개명 | 내부 |
| HS_STATS_FARM_DROP_INIT | 10879–10881 | M12 | C4 | `UI_NAV_STATS_FARM_INIT` 개명 | 내부 |
| HS_AGGVIZ_UPDATE_MOVED | 11086–11106 | M12 | C4 | `UI_NAV_AGGVIZ_UPDATE` 개명 | 내부 |
| HS_AGENT_UPDATE | 11473–11571 | M12 | C4 | `UI_AGENT_UPDATE` 개명 | |
| HS_FITTER_P1_1_AGENT_TRI_REPLACE | 11688–11715 | M12 | C4 | `UI_AGENT_TRI_REPLACE` 개명 | |
| PREDATOR_CLASS_CORE | 11737–12061 | M07 | 정상 | 유지 | 27.20 신설 |
| SOLITARY_ACTION_HELPERS | 12063–12165 | M07 | 정상 | 유지 | 27.20 신설 |
| HERBIVORE_CLASS_CORE | 12167–12407 | M07 | 정상 | 유지 | 27.20 신설 |
| ATLAS_P1_OVERVIEW_VARS | 12418–12423 | M11(권고) | C3 | render 측 이관 청사진 기록 | ⚠️ 아래 상세 |
| HS_A3_POP_RING_REPLACE | 12506–12590 | M10 | C4 | `POP_RING_CORE` 개명 | |
| HS_FITTER_P1_2_POP_SAMPLE | 12584–12589 | M10 | C4 | `POP_SAMPLE_CORE` 개명 | 내부 |
| ATLAS_P1_STATICMAP_CLIP | 12622–13185 | M11(권고) | C3 | render 측 이관 청사진 기록 | ⚠️ 아래 상세 |
| ATLAS_P1_OVERVIEW_CALL | 13208–13211 | M11(권고) | C3 | render 측 이관 청사진 기록 | 내부 호출 |
| ATLAS_P1_OVERVIEW_FN | 13214–13236 | M11(권고) | C3 | render 측 이관 청사진 기록 | ⚠️ 아래 상세 |
| SPAWN_AND_LOG_CORE | 13238–13613 | M10 | 정상 | 유지 | |
| HS_FITTER_P1_1_RENDERLOGS_GUARD | 13592–13597 | M10 | C4 | `SPAWN_RENDERLOGS_GUARD` 개명 | 내부 |
| SIM_COLLISION_CORE(현 HS_COLLISION_LOOP) | 13729–14740 | M10 | C1/C2 | `SIM_COLLISION_CORE` 개명 + 하위6블록 명시 | ISS-03 |
| HS_FITTER_P4_1_INPUT_BINDINGS | 14843–15144 | M13 | C4 | `UI_INPUT_BINDINGS` 개명 | |
| HS_HOTKEYS | 14855–14998 | M13 | C4 | `UI_HOTKEYS` 개명 | 내부 |
| HS_A3_KEYP_REPLACE | 14979–14996 | M13 | C4 | `UI_HOTKEYS_KEYP` 개명 | 내부 |
| ERA_BTN_SURNAME_AUTO_WIRE | 15147–15199 | M09 | 정상 | 유지 | |
| HS_FARM_OBS_MINI_BLOCKVIEW | 15350–15500 | M12 | C4 | `UI_FARM_OBS_BLOCKVIEW` 개명 | |
| HS_BRAIN_THROTTLE_HELPER | 15523–15560 | M10 | C4 | `SIM_BRAIN_THROTTLE` 개명 | |

### ⚠️ ATLAS_P1_* 내용 요약/귀속
- `ATLAS_P1_OVERVIEW_VARS`: overview canvas/context/scale 전역 변수만 선언.
- `ATLAS_P1_STATICMAP_CLIP`: 정적 지형 캔버스 레이어 합성/마스크/쉐이딩 등 렌더 파이프라인 코드.
- `ATLAS_P1_OVERVIEW_FN/CALL`: `bgCanvas`를 축소 렌더해 overview canvas 생성.

판단: 시뮬 로직보다 렌더 파이프라인 성격이 뚜렷하여 M11(render.js) 귀속이 합당. 현재 위치는 sim 덩어리 내에 있으므로 C3(이관 청사진) 기록.

### SIM_COLLISION 하위 6개 경계 대조
- `_hs3_layerResource` (14486): `SIM_COLLISION_EAT`
- `_hs3_layerHunt` (14512): `SIM_COLLISION_HUNT`
- `_hs3_layerAgentAgent` (14547): `SIM_COLLISION_AGENT_FIGHT/PEACE` (내부 분기)
- `_hs3_commitNewborns` (14662): `SIM_COLLISION_BIRTH_PEND`
- `_hs3_layerPredators` (14671): `SIM_COLLISION_PREDATOR`

판단: 함수 경계는 확정 하위 구조 6개와 개념적으로 정렬 가능. 단 `AGENT_FIGHT/PEACE`는 현재 단일 함수 내부 분기라 계층화 마커 신설 필요.

### GAP 경계 확인
- GAP-D: 13614–13728 (신설 예정 `FOOD_GRID_CORE`)
- GAP-E: 14741–14842 (신설 예정 `BRAIN_CANVAS_DRAW`)
- GAP-F: 15200–15349 (신설 예정 `UI_SYSTEM_PANELS_UPDATE`)

---

## S5 — UI_INSPECTOR_PANEL_UPDATE ~ HS_FITTER_P1_3_TREND_TAB_DIRTY

| 블록명 | 라인(현재) | 귀속 모듈 | 판정 | 권고 조치 | 비고 |
|---|---:|---|---|---|---|
| UI_INSPECTOR_PANEL_UPDATE | 15562–15722 | M12 | 정상 | 유지 | |
| HS_INSPECTOR_SUBST_FARM_DROP_ROW | 15654–15656 | M12 | C4 | `UI_INSPECTOR_FARM_DROP_ROW` 개명 | 내부 |
| HS_FITTER_P4_1_UITICK_REGION | 15724–15749 | M10 | C4 | `SIM_UITICK_REGION` 개명 | |
| HS_UITICK_WRAPPERS | 15726–15748 | M10 | C4 | `SIM_UITICK_WRAPPERS` 개명 | 내부 |
| HS_FITTER_P1_4_UITICK500_ROUTER | 15727–15743 | M10 | C4 | `SIM_UITICK500_ROUTER` 개명 | 내부 |
| HS_FITTER_P1_1_PANELVISIBLE | 15751–15761 | M12 | C4 | `UI_PANEL_VISIBLE` 개명 | |
| HS_FITTER_P1_2_HISTORY_UI | 15763–15803 | M12 | C4 | `UI_HISTORY_PANEL` 개명 | |
| HS_FITTER_P1_3_HISTORY_DRAW_CONTROL | 15779–15798 | M12 | C4 | `UI_HISTORY_DRAW_CTRL` 개명 | 내부 |
| HS_A3_DEV_PERF_API | 15890–15935 | M10 | C4 | `SIM_DEV_PERF_API` 개명 | |
| HS_FITTER_P4_1_RENDER_LOOP | 15955–16209 | M11 | C4 | `RENDER_LOOP_CORE` 개명 | |
| HS_PHASE1_HELPERS | 16214–17107 | M10 | C2/C4 | `SIM_PHASE1_HELPERS` 개명 + 분할 | ISS-05 |
| HS_FITTER_P4_1_SIM_LOOP | 16215–16904 | M10 | C4 | `SIM_LOOP_CORE` 개명 | 내부 |
| HS_19_SIMLOOP_DECAY_INSERT | 16390–16396 | M10 | C4 | `SIM_LOOP_DECAY` 개명 | 내부 |
| HS_AGRO_SELECTED_TILE_LAZY_TOUCH | 16398–16412 | M10 | 정상 | 유지 | 내부 로직 |
| AUTOCLAN_ANIMATE_HOOK | 16413–16421 | M10 | 정상 | 유지 | 내부 로직 |
| HS_RAF_SINGLETON_21_1 | 17109–17140 | M10 | C4 | `SIM_RAF_SINGLETON` 개명 | |
| HS_DISPOSE_WORLD_21_1 | 17142–17205 | M10 | C4 | `SIM_DISPOSE_WORLD` 개명 | |
| HS_MONITOR_CORE | 17236–17843 | M12 | C4 | `UI_MONITOR_CORE` 개명 | |
| DEV_CONSOLE_HELPERS | 17488–17671 | M12 | 정상 | 유지 | 내부 |
| HS_FITTER_P3_1_CORE_API | 17905–17936 | M12 | C4 | `UI_CORE_API` 개명 | |
| UI_DRAWERS_STEP1_INIT | 17938–18174 | M12 | 정상 | 유지 | 27.20 신설 |
| HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_LEGACY_STEP1 | 18097–18102 | M12 | C4 | `UI_DRAWERS_TOGGLERIGHT_STEP1` 개명 | legacy step1 |
| UI_DRAWER_TOGGLES_CORE | 18230–18412 | M12 | 정상 | 유지 | |
| HS_FITTER_P1_3_TOGGLERIGHT_DIRTY | 18255–18260 | M12 | C4 | `UI_DRAWERS_TOGGLERIGHT` 개명 | |
| HS_FITTER_P1_3_TREND_TAB_DIRTY | 18470–18479 | M12 | C4 | `UI_DRAWERS_TREND_TAB` 개명 | |

### ⚠️ GAP-G (15790–15875) 내용 요약/귀속
- 포함 요소: `_ensureFoodGridCurrent()`, `cachedFoodGrid*`, tick accumulator, perf/profile 표시 변수.
- `FOOD_GRID` 캐시 갱신은 food 데이터 접근이지만, 실행 맥락은 sim loop orchestration/성능측정 초기화에 결합.

판단:
- 귀속은 M10(sim.js)가 더 적절.
- 확정명 `FOOD_PHASE0_SIM_UTILS`는 의미가 혼합적이므로 C1 권고(예: `SIM_PHASE0_GRID_CACHE_UTILS`).

### HS_PHASE1_HELPERS(ISS-05) 분할 권고 유효성
- `HS_FITTER_P4_1_SIM_LOOP`(내부 689줄)와 주변 헬퍼(raf/perf/monitor 업데이트)의 책임이 다름.
- 1차 분리: `SIM_LOOP_CORE` / `SIM_RUNTIME_HELPERS` / `SIM_UI_SYNC_HELPERS` 3축 분할 권고 유효.

### GAP 경계 확인
- GAP-G: 15804–15889
- GAP-I: 18413–18469

---

## P2 — 전수조사 취합

### 1) 판정별 집계
- C1 (Rename): 6
  - CONFIG_TABS, SIM_COLLISION_CORE(현 HS_COLLISION_LOOP), FOOD_PHASE0_SIM_UTILS(GAP-G 명칭 재검토), UI 관련 일부 혼합명(세부는 S2/S4/S5 참조)
- C2 (Split/계층화): 7
  - WORLDGEN_UI_INIT, UI_INPUT_WORLD_UTILS, SIM_GLOBALS_POST_GAIA_ENV30, AGENT_GRID_CORE, UI_NAV_ROUTING, HS_PHASE1_HELPERS, SIM_COLLISION_CORE
- C3 (이관 청사진): 4
  - ATLAS_P1_OVERVIEW_VARS, ATLAS_P1_STATICMAP_CLIP, ATLAS_P1_OVERVIEW_CALL, ATLAS_P1_OVERVIEW_FN
- C4 (HS_ Rename): 52
  - HS_* 계열 전반 (S2~S5 표 참조)
- 정상(조치 불필요): 49
- ⚠️ 미확정 대상 최종 판단:
  - `UI_INPUT_WORLD_UTILS`: M12+M08 혼재(C2)
  - `SIM_GLOBALS_POST_GAIA_ENV30`: M10 귀속(C2)
  - `ATLAS_P1_*`: M11 귀속(C3)
  - `FOOD_PHASE0_SIM_UTILS`(GAP-G): M10 귀속(C1)

### 2) 우선순위 실행 순서 제안
- Phase 3-A (Rename + GAP fill, 비파괴)
  - C4 전량 rename
  - C1 rename 수행
  - GAP-D/E/F/G/I 블록 마커 신설
- Phase 3-B (Merge/Split)
  - C2 대상의 계층화/분할 마커 설계 및 순차 적용
- Phase 3-C (이관 청사진 문서화)
  - C3 대상(ATLAS_P1_*) 문서 이관 계획 기록(코드 이동 없음)

### 3) HARD_MODULE_BLUEPRINT 수정 필요 항목
- `SIM_GLOBALS_POST_GAIA_ENV30` 귀속을 M10으로 확정 반영 필요.
- `ATLAS_P1_*` 3개(+CALL)의 M11 귀속 확정 반영 필요.
- GAP-G(`FOOD_PHASE0_SIM_UTILS`) 귀속을 M10으로 확정하고 명칭 재검토 항목 추가 필요.
- `UI_INPUT_WORLD_UTILS`는 단일 귀속 대신 분할 권고(C2) 주석 추가 필요.

### 4) 버전 번프 판단
- 본 Phase1은 점검 전용이므로 버전 번프 불필요.
- 실제 rename/마커 추가 실행(Phase 3-A) 시 버전 번프 필요.

### 다음 스텝 체크리스트
1. BLUEPRINT의 ⚠️ 4개 항목 귀속 확정 반영.
2. Phase 3-A용 rename/GAP-fill 실행 프롬프트 작성.
3. Phase 3-B용 대형 블록 분할 기준(ISS-02/03/04/05) 상세화.
4. SNAPSHOT 라인번호 재동기화(이번 +14/+16 시프트 반영).
