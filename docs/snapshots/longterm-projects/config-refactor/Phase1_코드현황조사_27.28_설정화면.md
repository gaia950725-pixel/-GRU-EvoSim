# EvoSim 27.28 설정화면 Phase 1 코드현황조사 초안

기준 파일: `EvoSim_27.28_builder-block_GAP-finalize_FINAL.html`

작성 목적:
- 설정화면 패치 시 인게임 패널/카드 레이아웃 붕괴가 반복되는 원인을 끊기 위해, **현재 설정화면이 실제로 무엇을 읽고 무엇을 덮어쓰는지**를 코드 기준으로 전수조사한다.
- 다음 단계에서 `collect → normalize → validate → apply` 구조로 분리하기 위한 기초 자료를 만든다.

---

## 1. 핵심 관찰

1. 현재 시작 파이프라인은 `guardedStart()`에서 DOM을 직접 읽어 `CFG`를 덮어쓴다.
   - 직접 읽기 구간: `10486~10571`
   - 즉, 설정 UI 구조 변경이 곧 시뮬 시작 로직 변경으로 이어진다.

2. 설정화면에는 이미 “표시만 바꾸는 UI 동기화”와 “실제 규칙/값 결정”이 섞여 있다.
   - `_syncClimateUI`, `_syncAltUI`, `_syncMarUI`, `_syncMapDependentUI`
   - 관련 구간: `3377~3483`

3. 기본값은 두 층이 있다.
   - 엔진 기본값: `CFG` 초기값 (`3798~3873`)
   - 실질 시작 기본값: 설정 DOM의 initial value / checked / selected (`1876~2149`)
   - 둘이 불일치하는 항목이 존재한다. 대표적으로 `foodCount`, `foodEnergy`, `farmLandPct`.

---

## 2. 상태 소유권 분류(Phase 1 가설)

| 소유권 | 의미 |
|---|---|
| `SimulationConfig` | 시작 시점에 엔진 `CFG`로 반영되는 실제 시뮬 설정 |
| `DerivedPreviewState` | 설정화면 안에서만 보이는 파생 표시/보조 상태 |
| `ConfigUIState` | 설정 탭, touched 플래그 등 설정화면 자체의 UI 상태 |
| `EngineInternal` | 설정 UI에서 읽히지만 플레이어 설정이라기보다 엔진/개발값 성격이 강한 항목 |
| `NotAppliedYet` | UI는 있으나 현재 시작 파이프라인에 반영되지 않는 항목 |

---

## 3. 설정 항목 전수표

> 표의 “저장키”는 현재 코드 기준 **실제 반영 대상**을 뜻한다. 대부분 `CFG.*` 이며, 일부는 localStorage 또는 UI-only 상태다.

| DOM ID / name | 현재 UI 라벨 | 저장키 | 타입 | UI 기본값 | 엔진 기본값(CFG) | 적용시점 | 파생효과 / 보정 | 소유권 | 비고 |
|---|---|---:|---|---|---|---|---|---|---|
| `cfg-map` | 맵 형태 | `CFG.mapType` | enum | `TSL_LARGE` | `TSL_LARGE` | Initialize 시 | `_syncMapDependentUI()`가 river/mtn max·default 재계산. `guardedStart()`에서 worldWidth/worldHeight 및 preset seed 분기에도 사용 | `SimulationConfig` | 핵심 상위 설정 |
| `cfg-seed-lock` | 고정 토폴로지 | `CFG.presetSeedEnabled` | bool | `false` | 명시 초기값 없음(시작 시 주입) | Initialize 시 | seed가 켜져야만 `GAIA.earthTopo` special-case 허용 | `SimulationConfig` | 설정 UI와 월드젠 강결합 |
| `cfg-seed` | 고정 seed 값 | `CFG.presetSeed` | int | `12345` | 명시 초기값 없음(시작 시 주입) | Initialize 시 | seed-lock ON일 때만 실질 사용. preset/map preset seed와 경쟁 | `SimulationConfig` | `0`이면 비활성 취급 |
| `cfg-climate` | 기후 & 농경 시스템 | `CFG.climateEnabled` | bool | `true` | `true` | Initialize 시 | `_syncClimateUI()`가 river slider disabled, status text/color, farm label color 갱신 | `SimulationConfig` | climate OFF여도 DOM은 유지 |
| `cfg-river-count` | 강 수(하구) | `CFG.riverCount` | int | `3` (XL map이면 preview default 4로 재설정 가능) | `3` | Initialize 시 | mapType에 따라 max `6/8`. start에서 clamp. `farmLandPct = 0.36 * (riverCount / riverMax)` 재계산 | `SimulationConfig` | 실제 농경률 직접 입력이 아니라 파생값 |
| `cfg-altitude` | 고도 & 산맥 시스템 | `CFG.altitudeEnabled` | bool | `true` | 명시 초기값 없음(코드상 사용 존재) | Initialize 시 | `_syncAltUI()`가 mountain slider disabled, status text/color 갱신 | `SimulationConfig` | altitude OFF면 mountainCount 강제로 0 |
| `cfg-mtn-count` | 산맥 갯수 | `CFG.mountainCount` | int | `5` (XL map이면 max 8 유지) | 명시 초기값 검색 필요(직접 초기항목은 확인 못함) | Initialize 시 | mapType에 따라 max `6/8`. altitude OFF면 0, ON이면 `1..max` clamp | `SimulationConfig` | UI와 엔진 보정 둘 다 존재 |
| `cfg-maritime` | Maritime | `CFG.maritimeEnabled` | bool | `true` | `true` | Initialize 시 | `_syncMarUI()`가 status text/color만 갱신 | `SimulationConfig` | preview-only 부수효과는 작음 |
| `cfg-mut` | 신경망 돌연변이 확률 | `CFG.mutationRate` | float | `0.10` | `0.10` | Initialize 시 | 라이브 라벨 업데이트 | `SimulationConfig` | 세대변이 핵심 |
| `cfg-clan-mode` | 소속 산정 방식 | `CFG.clanMode` | enum | `SURNAME_AUTO` | `SURNAME_AUTO` | Initialize 시 | `TERRITORY` stale 값이면 `SURNAME`으로 normalize | `SimulationConfig` | UI 라벨과 실제 enum 의미 정리 필요 |
| `cfg-leader-mode` | 리더 시스템 | `CFG.leaderMode` | enum | `FOLLOW_BUFF` | `FOLLOW_BUFF` | Initialize 시 | start 후 `localStorage.leaderMode`에도 저장 | `SimulationConfig` | UI 문구상 “보류”인데 엔진 반영은 됨 |
| `cfg-pop` | 초기 인구 수 | `CFG.popSize` | int | `500` | `500` | Initialize 시 | 라이브 라벨 업데이트 | `SimulationConfig` | 명확 |
| `name="crisisSpeed"` | 인구위기 복원속도 | `CFG.crisisRecoverSpeed` | enum(float preset) | `1.0` (Normal) | `1.0` | Initialize 시 | `getCrisisRecoverSpeedFromUI()`가 `0.5/1.0/2.0` 외 값 차단 | `SimulationConfig` | DOM id 없음, radio group |
| `cfg-repro-age` | 번식 가능 나이(년) | `CFG.reproMinAgeYr`, `CFG.reproMinAgeTicks` | int | `12` | `12` | Initialize 시 | ticks로 2차 변환: `CAL_TICKS_PER_YR * reproMinAgeYr` | `SimulationConfig` | 파생 저장키 2개 |
| `nn-gru` | GRU | 없음 | bool(display) | checked + disabled | - | 미적용 | 표시용 | `NotAppliedYet` | 현재 아키텍처 선택 UI만 존재 |
| `nn-moe` | MoE-lite | 없음 | bool(display) | unchecked + disabled | - | 미적용 | 표시용 | `NotAppliedYet` | 미구현 |
| `nn-mlp` | MLP / FFN | 없음 | bool(display) | unchecked + disabled | - | 미적용 | 표시용 | `NotAppliedYet` | 미구현 |
| `cfg-foodcount` | 초기 식량 수 | `CFG.foodCount` | int | `2000` | `1000` | Initialize 시 | 라이브 라벨 업데이트 | `SimulationConfig` | **UI 기본값과 CFG 기본값 불일치** |
| `cfg-food` | 식량 획득 에너지 | `CFG.foodEnergy` | int | `750` | `736` | Initialize 시 | 라이브 라벨 업데이트 | `SimulationConfig` | **UI 기본값과 CFG 기본값 불일치** |
| `cfg-pred-species` | 맹수 종류 | `CFG.predatorSpecies` | enum | `SABERTOOTH`(첫 option) | `SABERTOOTH` | Initialize 시 | change 시 `lbl-pred-title` 텍스트 변경(맹수 수/팩 수) | `SimulationConfig` | species 따라 count 의미 변함 |
| `cfg-pred` | 맹수 수 / 팩 수 | `CFG.predatorCount` | int | `7` | `7` | Initialize 시 | species가 `DIREWOLF`이면 실제 spawn은 `Math.min(25, predatorCount)` 팩으로 해석 | `SimulationConfig` | 같은 slider지만 의미 가변 |
| `cfg-pred-bonus` | 맹수 사냥 보너스 | `CFG.predatorKillBonus` | int | `816` | `816` | Initialize 시 | UI hidden(devonly), slider disabled지만 start에서 그대로 읽음 | `EngineInternal` | 플레이어 설정이라기보다 엔진 상수 |
| `name="predThreat"` | 맹수 위협도 | `CFG.pred2_threatLevel` | int(enum) | `2` Normal | `2` | Initialize 시 | UI hidden(devonly), radios disabled지만 checked 값 읽음. start 후 `localStorage.pred2_threatLevel` 저장 | `EngineInternal` | DOM id 없음, radio group |
| `cfg-herb-species` | 초식동물 종류 | `CFG.herbivoreSpecies` | enum/string | `REINDEER` | `REINDEER` | Initialize 시 | upper-case normalize 후 `HERB_PROFILE` 존재 검사. 실패 시 fallback + select value 교정 | `SimulationConfig` | validate 경로가 이미 존재 |
| `cfg-herb-count` | 무리 수 | `CFG.herbivoreCount` | int | `30` | `30` | Initialize 시 | 라이브 라벨 업데이트 | `SimulationConfig` | 명확 |
| `cfg-herb-bonus` | 사냥 보너스 | `CFG.herbivoreBonus` | int | `500` | `500` | Initialize 시 | UI hidden(devonly), disabled지만 start에서 그대로 읽음 | `EngineInternal` | 플레이어 UI에서 제거 후보 |
| `cfg-herb-threat` | 위협도(공격력) | `CFG.herbivoreThreatLevel` | float | `1.0` | `1.0` | Initialize 시 | UI hidden(devonly), disabled지만 start에서 그대로 읽음 | `EngineInternal` | 플레이어 UI에서 제거 후보 |

---

## 4. 설정화면 내부의 UI-only / 파생 상태

이 항목들은 `CFG`에 바로 저장되지는 않지만, 설정화면 구조 변경 시 쉽게 깨질 수 있다.

| DOM / 상태 | 현재 역할 | 저장키 | 타입 | 기본값 | 적용시점 | 파생효과 / 보정 | 소유권 | 비고 |
|---|---|---:|---|---|---|---|---|---|
| `lbl-climate-status` | Climate ON/OFF 표시 | 없음 | text UI | `ON` | 즉시 | `_syncClimateUI()`가 text/color/weight 제어 | `DerivedPreviewState` | 엔진값 아님 |
| `lbl-altitude-status` | Altitude ON/OFF 표시 | 없음 | text UI | `ON` | 즉시 | `_syncAltUI()`가 text/color/weight 제어 | `DerivedPreviewState` | 엔진값 아님 |
| `lbl-maritime-status` | Maritime ON/OFF 표시 | 없음 | text UI | `ON` | 즉시 | `_syncMarUI()`가 text/color/weight 제어 | `DerivedPreviewState` | 엔진값 아님 |
| `lbl-river-count` | river 수 표시 | 없음 | text UI | `3` | 즉시 | mapType / slider 입력 따라 갱신 | `DerivedPreviewState` | 엔진 원본 아님 |
| `lbl-mtn-count` | mountain 수 표시 | 없음 | text UI | `5` | 즉시 | altitude on/off에 따라 색상/텍스트 변경 | `DerivedPreviewState` | 엔진 원본 아님 |
| `lbl-farm-pct` | 농경 가능 비율 표시 | 없음 | text UI | DOM 직접 확인 필요 | 즉시 | `0.36 * (riverCount / riverMax)`로 계산 | `DerivedPreviewState` | 실제 저장은 `CFG.farmLandPct` |
| `lbl-pop`, `lbl-foodcount`, `lbl-food`, `lbl-repro-age`, `lbl-pred`, `lbl-mut`, `lbl-pred-bonus`, `lbl-herb-count`, `lbl-herb-bonus`, `lbl-herb-threat` | 슬라이더 값 표시 | 없음 | text UI | 각 slider value와 동일 | 즉시 | slider `input` 이벤트로 갱신 | `DerivedPreviewState` | 순수 표시값 |
| `_uiTouchedRiver` | river 사용자가 건드렸는지 | 없음 | bool | `false` | 즉시 | map 변경 시 default 덮어쓰기 여부 결정 | `ConfigUIState` | 설정 저장이 아닌 session UI state |
| `_uiTouchedMtn` | mtn 사용자가 건드렸는지 | 없음 | bool | `false` | 즉시 | map 변경 시 default 덮어쓰기 여부 결정 | `ConfigUIState` | 설정 저장이 아닌 session UI state |
| `.cfg-tab` / `#cfg-tabs` | 설정 탭 전환 | `localStorage.EVOSIM_CFG_TAB` | string | world tab | 즉시/세션간 복원 | world/humanity/ecosystem 섹션 show/hide | `ConfigUIState` | 시작 CFG와 별개 |
| `cfg-side-desc` | 탭별 설명문 | 없음 | html UI | world 설명 | 즉시 | active tab에 따라 innerHTML 교체 | `DerivedPreviewState` | 패널과 분리 필요 |
| `cfg-apply-hint` | “다음 Initialize에만 적용” 안내 | 없음 | text UI | 고정 문자열 | 즉시 | 없음 | `DerivedPreviewState` | start-only 정책 표식 |

---

## 5. 현재 코드상 “실제 반영 순서” 요약

### A. 설정화면 입력 → 즉시 preview 반영
- `cfg-climate` → river slider enabled / status 표시 변경
- `cfg-altitude` → mtn slider enabled / status 표시 변경
- `cfg-maritime` → status 표시 변경
- `cfg-map` → `_syncMapDependentUI()`를 통해 river/mtn max/default, farm pct 재계산
- 각 slider → 대응 label text 즉시 갱신
- 탭 버튼 → section show/hide + localStorage 저장

### B. Initialize 버튼 / `guardedStart()`에서 실제 엔진 반영
1. population / food / reproduction / crisis speed
2. predator / mutation / map / clan / leader / species
3. herb species normalize & validate
4. climate / river / farmLandPct
5. altitude / maritime / mountainCount
6. localStorage 일부 저장 (`pred2_threatLevel`, `leaderMode`)
7. map preset size / seed / topo seed 분기

즉 현재 구조는 **preview용 상태와 engine apply가 분리되어 있지 않고, 단지 시점만 다르다.**

---

## 6. 확인된 불일치 / 리팩토링 우선 쟁점

### 6-1. 기본값 이중화와 불일치
- `CFG.foodCount = 1000` vs UI `cfg-foodcount = 2000`
- `CFG.foodEnergy = 736` vs UI `cfg-food = 750`
- `CFG.farmLandPct = 0.18` 이지만 실제 start 시 `riverCount` 기반으로 다시 계산됨

의미:
- “엔진 기본값”과 “실제 게임 시작 기본값”이 다르다.
- 따라서 설정화면 리디자인 이전에 **정본(default source of truth)** 을 정해야 한다.

### 6-2. hidden/disabled인데 start에서 읽는 값 존재
- `cfg-pred-bonus`
- `predThreat` radio
- `cfg-herb-bonus`
- `cfg-herb-threat`

의미:
- 플레이어가 못 바꾸는 값을 DOM에서 여전히 읽고 있다.
- 이 값들은 설정 UI 소유가 아니라 **엔진 상수/개발용 정책값**으로 분리하는 편이 낫다.

### 6-3. 하나의 control이 여러 의미를 가지는 경우
- `cfg-pred`는 species에 따라 “맹수 수” 또는 “팩 수”로 의미가 바뀐다.
- `cfg-river-count`는 단순 count가 아니라 `farmLandPct`의 입력축 역할도 한다.
- `cfg-repro-age`는 years 값을 입력하지만 실제 엔진에는 ticks까지 파생 저장한다.

의미:
- 다음 단계에서 `normalize/derive` 레이어가 반드시 필요하다.

### 6-4. 설정 UI 내부 상태가 start 결과에 간접 영향
- `_uiTouchedRiver`, `_uiTouchedMtn` 때문에 map 변경 시 default overwrite 여부가 달라진다.

의미:
- 같은 DOM이라도 “설정값”과 “설정화면 상호작용 흔적”은 분리 저장해야 한다.

---

## 7. Phase 2로 넘길 조사 질문

1. `mountainCount`의 엔진 기본 초기값을 CFG 선언부 어디까지 추적할지 추가 확인 필요.
2. `lbl-farm-pct` DOM 선언부와 표시 기본 텍스트를 정확히 캡처해 둘 필요가 있다.
3. `predatorKillBonus`, `pred2_threatLevel`, `herbivoreBonus`, `herbivoreThreatLevel`을 플레이어 설정에서 완전히 제거할지, Dev 전용 설정 레이어로 분리할지 정책 결정 필요.
4. `leaderMode`, `pred2_threatLevel`만 localStorage에 쓰는 현재 방식이 유지될지 검토 필요.
5. `cfg-map` 변경이 월드젠 외 렌더/패널 레이아웃 CSS에 영향을 주는 결합지점이 있는지 별도 조사 필요.

---

## 8. 다음 단계 권고

권고 순서:
1. **정본 기본값 표 확정** (`CFG 기본값` vs `UI 기본값` 중 무엇을 기준으로 할지)
2. **설정 항목을 세 부류로 재분류**
   - Player-facing SimulationConfig
   - EngineInternal / DevOnly
   - UI-only preview state
3. **`guardedStart()`에서 DOM 직독을 제거**하고 아래 4단계로 분리
   - `collectSimulationConfigFromUI()`
   - `normalizeSimulationConfig(raw)`
   - `validateSimulationConfig(cfg)`
   - `applySimulationConfigToEngine(cfg)`

이 문서는 그 분리를 위한 Phase 1 조사 초안이다.
