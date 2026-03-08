# EvoSim 27.28 설정화면 리팩토링 Phase 2 청사진 (재작성본)

기준 파일: `EvoSim_27.28_builder-block_GAP-finalize_FINAL.html`
기준 조사문서: `Phase1_코드현황조사_27.28_설정화면.md`
중요도(1차 의견): **중요**

---

## 0. 문서 목적

이 문서는 설정화면 패치를 할 때마다 인게임 패널/카드가 같이 무너지는 문제를 끊기 위해,
**설정값 수집과 시뮬 시작 파이프라인을 분리하는 구조 청사진**을 정의한다.

핵심 목적은 두 가지다.

1. `guardedStart()`에서 DOM 직독을 제거한다.
2. 설정 UI / 설정 데이터 / 인게임 패널 상태 / 월드젠 준비를 서로 다른 소유권으로 분리한다.

이 Phase 2는 **디자인 변경 문서가 아니다.**
UI 리디자인은 이 구조 분리 이후에만 진행한다.

---

## 1. 현재 코드의 구조적 문제 요약

27.28 기준 현재 구조는 다음 특징을 가진다.

### 1-1. `guardedStart()`가 너무 많은 역할을 맡고 있다
현재 `guardedStart()`는 아래를 한 함수에서 모두 처리한다.

- DOM에서 설정값 읽기
- 타입 변환 / clamp / fallback
- 일부 DOM 되쓰기 (`cfg-herb-species`, `cfg-mtn-count`)
- localStorage 쓰기 (`pred2_threatLevel`, `leaderMode`)
- `CFG.*` 반영
- map preset / seed / `GAIA.rand` / `GAIA.earthTopo` 준비

즉, 지금의 `guardedStart()`는 단순 시작 트리거가 아니라
**설정 수집기 + 정규화기 + UI 보정기 + 세션 저장기 + 엔진 적용기 + 월드젠 브리지**다.

### 1-2. 설정화면에는 이미 “UI 상태”와 “실제 시뮬 설정”이 섞여 있다
대표 사례:

- `_uiTouchedRiver`, `_uiTouchedMtn`
- `_syncClimateUI()`, `_syncAltUI()`, `_syncMarUI()`, `_syncMapDependentUI()`
- `EVOSIM_CFG_TAB` localStorage 기반 탭 복원

이들은 모두 설정화면 동작에는 필요하지만,
그 자체가 시뮬레이션 설정값은 아니다.

### 1-3. hidden / disabled control을 아직도 start 시점에 읽는다
대표 항목:

- `cfg-pred-bonus`
- `predThreat`
- `cfg-herb-bonus`
- `cfg-herb-threat`

즉 현재는 **플레이어 설정 / 개발용 상수 / 내부 엔진 정책값**이 명확히 분리되어 있지 않다.

### 1-4. 인게임 패널은 원래 별도 상태기계인데 설정 패치에 자주 같이 휘말린다
- `window.__HS_UI`
- inspector/history/info panel 상태
- localStorage 기반 drawer/collapse/tabs

이 레이어는 설정화면 리팩토링과 분리되어야 한다.

---

## 2. 이번 Phase 2의 설계 원칙

### 원칙 A. `guardedStart()`는 오케스트레이터만 맡는다
최종적으로 `guardedStart()`는
“값 수집 → 정규화 → 검증 → 엔진 반영 → 월드젠 준비 → 시작”의 호출 순서만 가진다.

### 원칙 B. “설정값”과 “UI 흔적”을 분리한다
아래 둘은 같은 DOM에서 오더라도 다른 객체에 저장한다.

- 실제 시뮬에 반영될 설정값
- UI 상호작용 흔적 (`touched`, active tab, preview text 등)

### 원칙 C. `apply`를 하나의 거대 단계로 두지 않는다
외부 개념은 `collect / normalize / validate / apply`로 유지하되,
내부 구현은 최소 아래 6단계로 쪼갠다.

1. `collect`
2. `normalize`
3. `validate`
4. `reconcileUI`
5. `applyCFG`
6. `prepareWorldgen`

즉, 기존 `apply`를 **UI 보정 / CFG 반영 / 월드젠 준비**로 쪼갠다.

### 원칙 D. 인게임 패널 상태는 설정 파이프라인 밖에 둔다
설정화면 리팩토링은 다음을 직접 건드리지 않는다.

- `__HS_UI`
- history / inspector / info panel 상태기계
- `.panel` 공통 동작
- drawer collapse / visible 상태

### 원칙 E. 기본값 정본은 하나만 둔다
지금처럼 `CFG 기본값`과 `DOM 초기값`이 갈라진 상태를 끝낸다.
최종 정본은 `EvoSim.config.defaults` 또는 동등한 단일 테이블 하나로 통일한다.

---

## 3. 목표 구조: 상태 레이어 5분할

이번 재작성본은 상태를 5개 레이어로 나눈다.

### 3-1. `SimulationConfig`
플레이어가 설정하고, 실제 시뮬 시작 시 `CFG`에 반영되는 값.

예:
- `mapType`
- `presetSeedEnabled`
- `presetSeed`
- `climateEnabled`
- `riverCount`
- `altitudeEnabled`
- `mountainCount`
- `maritimeEnabled`
- `mutationRate`
- `clanMode`
- `leaderMode`
- `popSize`
- `crisisRecoverSpeed`
- `reproMinAgeYr`
- `foodCount`
- `foodEnergy`
- `predatorSpecies`
- `predatorCount`
- `herbivoreSpecies`
- `herbivoreCount`

### 3-2. `DerivedSimulationConfig`
시뮬 입력이지만, UI에서 직접 입력받지 않고 정규화/파생을 통해 계산되는 값.

예:
- `reproMinAgeTicks`
- `farmLandPct`
- species fallback 이후 확정된 `herbivoreSpecies`
- mapType 기준 최대 river/mountain 범위

### 3-3. `ConfigUIState`
설정 화면의 상호작용 흔적과 레이아웃 상태.

예:
- `touched.river`
- `touched.mountain`
- `activeTab`
- `lastMapType`
- `isHydratingDefaults`
- `isApplyingPreset`

### 3-4. `ConfigPreviewState`
화면에만 쓰는 표시용 파생 상태.

예:
- `climateStatusText`
- `altitudeStatusText`
- `maritimeStatusText`
- `farmPctLabel`
- slider label text
- predator count label title

### 3-5. `EngineInternalConfig`
플레이어-facing 설정이 아니라 엔진/개발용 정책값.

예:
- `predatorKillBonus`
- `pred2_threatLevel`
- `herbivoreBonus`
- `herbivoreThreatLevel`

> 권장 방침: 이 레이어는 향후 player config에서 분리한다.
> 다만 Phase 3 초기 구현에서는 회귀 최소화를 위해 **현재 동작을 보존하는 범위에서 별도 소유권만 분리**한다.

---

## 4. 소유권 규칙

| 레이어 | 주 소유자 | 저장 위치 | 비고 |
|---|---|---|---|
| `SimulationConfig` | config pipeline | `EvoSim.config.simulation` | start 입력의 정본 |
| `DerivedSimulationConfig` | normalize/derive 단계 | `EvoSim.config.derived` | UI 직접 편집 금지 |
| `ConfigUIState` | config UI helper | `EvoSim.config.uiState` | start에 직접 반영 금지 |
| `ConfigPreviewState` | preview sync helper | 메모리/DOM | 세션 표시 전용 |
| `EngineInternalConfig` | engine/dev layer | `EvoSim.config.internal` 또는 `CFG` defaults bridge | player-facing와 분리 |
| `PanelLayoutState` | in-game drawer system | `window.__HS_UI` | 이번 패치 범위 밖 |

---

## 5. 파이프라인 청사진 (재작성본)

이전 청사진의 문제는 `apply`가 너무 넓었다는 점이다.
이번 재작성본은 실제 구현 파이프라인을 아래처럼 고정한다.

## 5-0. 부트 시점: `hydrateConfigUIFromDefaultsAndPrefs()`

**시점:** 페이지 로드 직후 / config overlay 초기화 시

**역할:**
- 정본 defaults를 기준으로 DOM 초기화
- localStorage 기반 UI preference 복원 (`activeTab`, 필요 시 `leaderMode`, `predThreat`)
- `_uiTouchedRiver`, `_uiTouchedMtn` 초기화
- 최초 preview sync 호출

**주의:**
- 이 단계는 시뮬 시작이 아니다.
- `CFG` 반영 금지
- worldgen 준비 금지

---

## 5-1. `collectRawConfigFromUI()`

**입력:** 현재 DOM
**출력:** `RawConfigSnapshot`

**역할:**
- DOM에서 문자열/체크 상태/라디오 값을 읽는다.
- 타입 변환, clamp, fallback은 하지 않는다.
- `SimulationConfig` 후보와 `EngineInternalConfig` 후보를 분리해 수집한다.

**규칙:**
- DOM 읽기는 여기로 집중한다.
- `guardedStart()` 포함 다른 위치에서 `document.getElementById('cfg-*')`를 직접 읽지 않는다.
- 기존 select/radio 다중 경로(`clanMode`, `leaderMode`)는 collect 단계에서만 처리한다.

**예시 출력 형태:**

```js
{
  simulationRaw: {
    mapType: "TSL_LARGE",
    presetSeedEnabled: false,
    presetSeed: "12345",
    climateEnabled: true,
    riverCount: "3",
    altitudeEnabled: true,
    mountainCount: "5",
    maritimeEnabled: true,
    mutationRate: "0.10",
    clanMode: "SURNAME_AUTO",
    leaderMode: "FOLLOW_BUFF",
    popSize: "500",
    crisisRecoverSpeed: "1.0",
    reproMinAgeYr: "12",
    foodCount: "2000",
    foodEnergy: "750",
    predatorSpecies: "SABERTOOTH",
    predatorCount: "7",
    herbivoreSpecies: "REINDEER",
    herbivoreCount: "30"
  },
  internalRaw: {
    predatorKillBonus: "816",
    pred2_threatLevel: "2",
    herbivoreBonus: "500",
    herbivoreThreatLevel: "1.0"
  }
}
```

---

## 5-2. `normalizeConfig(raw)`

**입력:** `RawConfigSnapshot`
**출력:** `NormalizedConfig`

**역할:**
- 타입 변환
- stale enum fallback
- species fallback
- mapType 기반 river/mountain 범위 계산
- `DerivedSimulationConfig` 계산

**여기서 수행할 항목:**
- `parseInt`, `parseFloat`, `checked -> bool`
- `clanMode === 'TERRITORY'` → `SURNAME`
- invalid `herbivoreSpecies` → fallback
- `riverCount` clamp
- `mountainCount` clamp
- altitude OFF면 `mountainCount = 0`
- `reproMinAgeTicks = CAL_TICKS_PER_YR * reproMinAgeYr`
- `farmLandPct = 0.36 * (riverCount / riverMax)`

**여기서 하지 않을 것:**
- DOM 되쓰기 금지
- localStorage 쓰기 금지
- `CFG` 반영 금지
- `GAIA.*` 준비 금지

즉 normalize는 **계산/보정만** 담당한다.

---

## 5-3. `validateNormalizedConfig(normalized)`

**입력:** `NormalizedConfig`
**출력:** `{ ok, fatalErrors, warnings }`

**역할:**
- 실제 시작 불가 상태와 경고 상태를 구분한다.

### fatal 예시
- mapType이 최종적으로 확정되지 않음
- collect 필수 control 부재로 핵심 값이 생성 불가
- normalize 후에도 필수 species / enum이 비어 있음

### warning 예시
- herb species fallback 발생
- stale clan mode가 normalize됨
- count 값이 범위를 넘어 clamp됨

**원칙:**
현재 코드가 fallback/clamp로 처리하던 항목을 무조건 fatal로 올리지 않는다.
현재 27.28의 의미론을 가능한 한 유지한다.

---

## 5-4. `reconcileConfigUIFromNormalized(normalized)`

**입력:** `NormalizedConfig`
**출력:** 없음 (DOM/UI state만 수정)

이 단계는 이전 청사진에서 빠졌던 핵심 단계다.

**역할:**
- normalize 결과를 DOM에 되써서 UI와 실제 적용값을 맞춘다.
- preview label/status를 normalized 기준으로 다시 그린다.
- touched 정책을 보존한 채 map-dependent UI를 갱신한다.

**대표 작업:**
- fallback된 `cfg-herb-species`를 select에 반영
- clamp된 `cfg-mtn-count`, `cfg-river-count`를 slider에 반영
- climate/altitude/maritime 상태 라벨 재동기화
- farm % 라벨 재동기화
- predator title / count 의미 표시 유지

**원칙:**
UI 되쓰기는 normalize가 아니라 **reconcileUI**가 맡는다.

---

## 5-5. `applyNormalizedConfigToCFG(normalized)`

**입력:** `NormalizedConfig`
**출력:** 없음 (`CFG`만 수정)

**역할:**
- `SimulationConfig`와 `DerivedSimulationConfig`를 `CFG`에 반영한다.
- 이 단계는 `CFG.*` 대입만 담당한다.

**포함 예시:**
- `CFG.popSize`
- `CFG.foodCount`
- `CFG.foodEnergy`
- `CFG.reproMinAgeYr`
- `CFG.reproMinAgeTicks`
- `CFG.predatorCount`
- `CFG.mutationRate`
- `CFG.mapType`
- `CFG.clanMode`
- `CFG.leaderMode`
- `CFG.predatorSpecies`
- `CFG.herbivoreSpecies`
- `CFG.herbivoreCount`
- `CFG.climateEnabled`
- `CFG.riverCount`
- `CFG.farmLandPct`
- `CFG.altitudeEnabled`
- `CFG.maritimeEnabled`
- `CFG.mountainCount`

**주의:**
- localStorage 쓰기 금지
- DOM 수정 금지
- map preset / `GAIA.*` 준비 금지

---

## 5-6. `applyInternalConfig(normalizedInternal)`

**입력:** `EngineInternalConfig`
**출력:** 없음

**역할:**
- 개발용/엔진용 설정값만 따로 반영한다.

**포함 예시:**
- `CFG.predatorKillBonus`
- `CFG.pred2_threatLevel`
- `CFG.herbivoreBonus`
- `CFG.herbivoreThreatLevel`

**정책:**
이 레이어는 장기적으로 player config와 분리해야 한다.
다만 27.28 회귀 방지를 위해 초기 단계에서는 별도 apply 함수로 분리만 먼저 한다.

---

## 5-7. `persistConfigPreferences(normalized)`

**입력:** `NormalizedConfig`
**출력:** 없음

**역할:**
- UI preference 또는 세션 재사용 목적의 localStorage 갱신을 담당한다.

**포함 예시:**
- `leaderMode`
- `pred2_threatLevel`
- 필요 시 `activeTab`

**주의:**
이 단계는 `CFG apply`와 분리한다.
이전 구조처럼 `guardedStart()` 중간에서 흩어 쓰지 않는다.

---

## 5-8. `prepareWorldgenRuntimeFromConfig(normalized, CFG)`

**입력:** 최종 normalized config + 이미 반영된 `CFG`
**출력:** worldgen/runtime 준비 상태

**역할:**
- `worldWidth`, `worldHeight` 결정
- preset size override
- preset seed / topo seed / `GAIA.rand` 준비
- `GAIA.earthTopo`, `_seedBase`, `_seedMtn`, `_seedRiver` 설정

**중요:**
이 단계는 설정 적용(`applyCFG`)과 별도다.
즉 월드젠 준비는 설정 수집/보정의 일부가 아니라
**시작 파이프라인의 후반 브리지**로 본다.

---

## 5-9. 최종 `guardedStart()` 목표 형태

최종적으로 `guardedStart()`는 아래 형태를 목표로 한다.

```js
async function guardedStart(){
  if (WORLDGEN.state === 'generating') return;

  setWorldGenState('generating', 'prepare', 'Reading configuration...', 0.05);
  await nextFrame();

  const raw = collectRawConfigFromUI();
  const normalized = normalizeConfig(raw);
  const validation = validateNormalizedConfig(normalized);
  if (!validation.ok) {
    abortStartWithConfigErrors(validation);
    return;
  }

  reconcileConfigUIFromNormalized(normalized);
  applyNormalizedConfigToCFG(normalized);
  applyInternalConfig(normalized.internal);
  persistConfigPreferences(normalized);
  prepareWorldgenRuntimeFromConfig(normalized, CFG);

  // 이후 worldgen / start orchestration만 수행
}
```

즉 `guardedStart()`는 더 이상 DOM 세부사항이나 개별 보정 규칙을 모르면 된다.

---

## 6. 정본(default source of truth) 정책

### 목표
기본값은 한 군데만 고친다.

### 권장 구조

```js
EvoSim.config.defaults = {
  simulation: { ... },
  internal: { ... },
  uiState: { ... }
};
```

### 적용 원칙
- DOM initial value는 defaults에서 hydrate한다.
- `CFG_DEFAULTS_START`는 장기적으로 defaults와 정합되는 bridge 역할만 수행한다.
- 하드코딩된 DOM value/checked는 점진적으로 제거한다.

### 이행 순서
1. defaults 테이블 작성
2. DOM hydrate 함수 작성
3. preview sync가 defaults/normalized를 참조하게 변경
4. 이후 `guardedStart()` DOM 직독 제거

즉 defaults 통일은 선언만이 아니라 **이행 순서**까지 포함해야 한다.

---

## 7. 관련 BUILDER_BLOCK 범위

이번 구조 개편과 직접 관련된 블록은 아래다.

### 직접 변경 후보
- `UI_CONFIG_START`
- `WORLDGEN_UI_INIT_START`
- `CONFIG_TABS_START`
- `CFG_DEFAULTS_START`
- `LOCALSTORAGE_RESTORE_START`
- `UI_STARTUP_CONTROLS_INIT_START`
- `UI_NAV_ROUTING_START` (`guardedStart()` 포함)
- `MAP_PRESETS_START`

### 원칙상 수정 금지(이번 Phase 2 범위 밖)
- `UI_DRAWERS_STEP1_INIT_START`
- `UI_DRAWER_TOGGLES_CORE_START`
- `UI_HISTORY_PANEL_START`
- `UI_INSPECTOR_PANEL_UPDATE_START`
- `UI_SYSTEM_PANELS_UPDATE_START`
- `UI_PANEL_VISIBLE_START`

즉 설정 파이프라인 개편은 **config overlay와 start pipeline 중심**으로 제한한다.

---

## 8. 비목표(이번 Phase 2에서 하지 않을 것)

다음은 이번 단계에서 하지 않는다.

1. 설정화면 카드 디자인 교체
2. 인게임 패널 미관/레이아웃 개편
3. player-facing 옵션 대량 추가
4. `data-cfg-*` 전면 도입
5. hidden/devonly 옵션 완전 제거
6. 설정화면과 인게임 패널 CSS 통합 리디자인

> 이유: 지금 필요한 것은 “보기 좋은 구조”가 아니라 “깨지지 않는 논리 분리”다.

---

## 9. 정책 필요 항목

### 정책 A. hidden/devonly 설정의 장기 소유권
선택지:
- **A안(권장):** `EngineInternalConfig`로 분리하고 player config에서는 사실상 제외
- B안: 현행처럼 설정 DOM에 남기되, dev 섹션으로 명시 유지

권장: **A안**
이유: 지금 문제는 player config와 engine/dev 상수가 섞여 있다는 점이다.

### 정책 B. `leaderMode`, `pred2_threatLevel` localStorage 유지 여부
선택지:
- **A안(권장):** 유지하되 `persistConfigPreferences()`로 이동
- B안: localStorage 자체 폐기

권장: **A안**
이유: 기존 UX를 깨지 않으면서도 책임 위치만 분리할 수 있다.

### 정책 C. `data-cfg-*` 도입 시점
선택지:
- **A안(권장):** Phase 3 이후 2차 패치
- B안: 이번에 함께 도입

권장: **A안**
이유: 지금은 DOM 직독 집중화가 먼저다. 바인딩 시스템까지 한 번에 넣으면 범위가 커진다.

---

## 10. 성공 조건

이번 Phase 2가 성공하려면 아래가 성립해야 한다.

1. `guardedStart()` 안에 `cfg-*` 직독이 남지 않는다.
2. normalize가 DOM을 직접 수정하지 않는다.
3. UI 되쓰기는 `reconcileConfigUIFromNormalized()` 한 군데로 모인다.
4. `CFG` 반영은 `applyNormalizedConfigToCFG()`로 집중된다.
5. worldgen seed/preset 준비는 별도 단계로 분리된다.
6. 인게임 패널 상태(`__HS_UI`)는 설정 파이프라인에서 건드리지 않는다.

---

## 11. 다음 스텝

권장 다음 스텝은 **Phase 3 작업명세서**다.

그 작업명세서는 최소 아래를 포함해야 한다.

- 대상 BUILDER_BLOCK별 수정 범위
- 신규 함수 시그니처
- `guardedStart()`에서 이관할 기존 코드 구간
- 회귀 검증 체크리스트
- localStorage / defaults / preview sync 이행 순서

즉 이번 Phase 2는 “어떻게 쪼갤 것인가”를 확정한 문서이고,
다음 문서는 “어느 줄을 어디로 옮길 것인가”를 확정하는 문서가 되어야 한다.
