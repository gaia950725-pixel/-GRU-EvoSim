# EvoSim Phase 3 Task Specification
## Config Pipeline Refactor for v27.28

Base file: `EvoSim_27.28_builder-block_GAP-finalize_FINAL.html`  
Base references: `Phase1_코드현황조사_27.28_설정화면.md`, `Phase2_Config_Pipeline_Rebuild_v27_28.md`  
Importance (first-pass): **중요**

---

## 0. 목표

이번 패치의 목표는 설정화면 리팩토링을 “디자인 수정”이 아니라 **시작 파이프라인 분리 패치**로 수행하는 것이다.

최종 목표는 아래 3개다.

1. `guardedStart()`에서 설정 DOM 직독을 제거한다.
2. 설정값 / UI 상태 / preview 상태 / 내부 엔진 설정 / 월드젠 준비를 분리한다.
3. 이후 설정 카드 레이아웃 변경이 인게임 패널/카드 붕괴로 이어지지 않도록, 설정 관련 로직의 진입점을 한 군데로 고정한다.

---

## 1. 비목표(이번 패치에서 하지 않을 것)

이번 패치에서는 아래를 하지 않는다.

- 설정화면 카드 디자인 리디자인
- 인게임 패널(`info/history/inspector`) 구조 변경
- `__HS_UI` 상태기계 수정
- `.panel` 공통 동작 수정
- worldgen 알고리즘 자체 변경
- slider 수치 밸런스 변경
- `data-cfg-*` 전면 치환

즉 이번 패치는 **논리 재배선**이 목적이지, UI 미감 변경 패치가 아니다.

---

## 2. 변경 범위

### 2-1. 대상 파일
- `EvoSim_27.28_builder-block_GAP-finalize_FINAL.html`

### 2-2. 우선 수정 대상 BUILDER_BLOCK
- `WORLDGEN_UI_INIT_START`
- `CONFIG_TABS_START` (직접 구조 변경은 최소화, 필요 시 연계만)
- `CFG_DEFAULTS_START`
- `UI_STARTUP_CONTROLS_INIT_START`
- `LOCALSTORAGE_RESTORE_START`
- `UI_NAV_ROUTING_START` (`guardedStart()` 포함)

### 2-3. 신규 BUILDER_BLOCK 추가 권장
아래 블록을 신규 추가한다. 기존 마커 밖 신규 추가가 필요하다.

1. `CONFIG_PIPELINE_CORE`
2. `CONFIG_PIPELINE_COLLECT`
3. `CONFIG_PIPELINE_NORMALIZE`
4. `CONFIG_PIPELINE_VALIDATE`
5. `CONFIG_PIPELINE_RECONCILE`
6. `CONFIG_PIPELINE_APPLY`
7. `CONFIG_PIPELINE_WORLDGEN`

권장 위치:
- `CFG_DEFAULTS_START` 직후 또는 `UI_STARTUP_CONTROLS_INIT_START` 직전
- 이유: 설정 관련 정본/보조 함수/시작 파이프라인이 가까이 있어야 추적이 쉽다.

---

## 3. 현재 코드에서 분리해야 하는 실제 구간

### 3-1. `guardedStart()` 직접 읽기/보정/적용 구간
실제 분리 대상은 `guardedStart()` 초반부의 아래 책임 묶음이다.

- DOM에서 `cfg-*` 읽기
- `clanMode`, `leaderMode` select/radio fallback 읽기
- `HERB_PROFILE` 기준 species fallback
- `riverCount`, `mountainCount` clamp
- `farmLandPct` 파생 계산
- `localStorage` 쓰기
- `disp-clan-mode` UI 쓰기
- `worldWidth/worldHeight` preset 반영
- preset seed / `GAIA.rand` / `GAIA.earthTopo` 준비

이번 패치에서는 **`guardedStart()` 내부에서 위 구간을 제거하고 함수 호출로 치환**한다.

### 3-2. UI preview 동기화 구간
현재 아래 로직은 설정 preview/interaction 전용이다.

- `_syncClimateUI()`
- `_syncAltUI()`
- `_syncMarUI()`
- `_syncMapDependentUI()`
- `_uiTouchedRiver`
- `_uiTouchedMtn`

이 구간은 start pipeline과 분리된 채 유지하되, 이후 `reconcileConfigUI()`가 필요 시 이 로직을 재호출할 수 있도록 정리한다.

### 3-3. localStorage restore 구간
현재 아래 2가지는 부트 시점 restore로 남아 있다.

- `pred2_threatLevel`
- `leaderMode`

이번 패치에서는 “부트 시점 restore”와 “start 시점 persist”의 책임을 같은 파이프라인 문맥으로 묶되, 실제 호출 시점은 분리한다.

---

## 4. 새 상태 구조

이번 패치는 설정 관련 상태를 아래 5계층으로 나눈다.

### 4-1. `EvoSim.config.defaults`
설정 정본. DOM 초기값보다 우선한다.

예시 필드:
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
- internal defaults (`predatorKillBonus`, `pred2_threatLevel`, `herbivoreBonus`, `herbivoreThreatLevel`)

### 4-2. `EvoSim.config.uiState`
설정화면 상호작용 흔적.

필수 필드:
- `touchedRiver`
- `touchedMountain`
- `activeTab`
- `lastMapType`
- `isHydratingDefaults`
- `isApplyingPreset`

### 4-3. `EvoSim.config.raw`
DOM에서 읽은 가공 전 snapshot.

### 4-4. `EvoSim.config.normalized`
normalize 완료 후 start에 사용할 값.

### 4-5. `EvoSim.config.derived`
파생값.

필수 필드:
- `reproMinAgeTicks`
- `farmLandPct`
- `riverMax`
- `mountainMax`
- `resolvedHerbivoreSpecies`
- `resolvedWorldWidth`
- `resolvedWorldHeight`
- `resolvedSeed`
- `earthTopoEnabled`

---

## 5. 신규 함수 명세

## 5-0. 공통 규칙
- DOM 직독은 `collectRawConfigFromUI()` 안에서만 허용한다.
- `CFG` 직접 쓰기는 `applyNormalizedConfigToCFG()` 안에서만 허용한다.
- `GAIA.rand`, `GAIA.earthTopo`, `GAIA._seed*` 변경은 `prepareWorldgenFromNormalizedConfig()` 안에서만 허용한다.
- DOM 되쓰기는 `reconcileConfigUI()` 안에서만 허용한다.
- localStorage 쓰기는 `persistConfigPreferences()` 안에서만 허용한다.

### 5-1. `ensureConfigNamespace()`

역할:
- `EvoSim.config` 네임스페이스가 없으면 생성
- `defaults/uiState/raw/normalized/derived` 기본 객체 생성

의사코드:

```js
function ensureConfigNamespace(){
  EvoSim.config = EvoSim.config || {};
  EvoSim.config.defaults = EvoSim.config.defaults || {};
  EvoSim.config.uiState = EvoSim.config.uiState || {};
  EvoSim.config.raw = EvoSim.config.raw || null;
  EvoSim.config.normalized = EvoSim.config.normalized || null;
  EvoSim.config.derived = EvoSim.config.derived || null;
}
```

### 5-2. `buildConfigDefaultsFromCFG()`

역할:
- 현 `CFG` 값을 기준으로 설정 정본 테이블 생성
- 1차 패치에서는 `CFG`를 source-of-truth로 사용해 회귀를 최소화
- 이후 단계에서 DOM 초기값 의존을 줄인다

주의:
- 이번 패치에서 `CFG` 구조를 대대적으로 재배열하지 않는다
- 우선 `defaults`를 생성하고 hydrate/collect/normalize가 이를 참조하게 만든다

### 5-3. `hydrateConfigUIFromDefaultsAndPrefs()`

호출 시점:
- 페이지 부트 직후
- config overlay 초기화 직후

역할:
- DOM에 정본 defaults 반영
- `pred2_threatLevel`, `leaderMode`, `EVOSIM_CFG_TAB` restore 반영
- `uiState.touched* = false` 초기화
- `_syncMapDependentUI()` 1회 실행

주의:
- `CFG` 변경 금지
- `GAIA` 변경 금지
- session start 금지

### 5-4. `collectRawConfigFromUI()`

입력:
- 현재 DOM

출력 예시:

```js
{
  simulationRaw: {
    popSize: "500",
    foodCount: "1000",
    foodEnergy: "736",
    reproMinAgeYr: "12",
    crisisRecoverSpeed: "1.0",
    predatorCount: "7",
    mutationRate: "0.10",
    mapType: "TSL_LARGE",
    clanMode: "SURNAME_AUTO",
    leaderMode: "FOLLOW_BUFF",
    predatorSpecies: "SABERTOOTH",
    herbivoreSpecies: "REINDEER",
    herbivoreCount: "30",
    climateEnabled: true,
    riverCount: "3",
    altitudeEnabled: true,
    mountainCount: "5",
    maritimeEnabled: true,
    presetSeedEnabled: false,
    presetSeed: "0"
  },
  internalRaw: {
    predatorKillBonus: "816",
    pred2_threatLevel: "2",
    herbivoreBonus: "500",
    herbivoreThreatLevel: "1.0"
  }
}
```

필수 세부 지시:
- `clanMode`, `leaderMode`는 현재와 동일하게 `select -> checked radio -> fallback default` 순으로 읽는다.
- `predThreat`도 현재와 동일하게 radio에서 읽는다.
- 누락 DOM은 즉시 throw하지 말고 raw에 `null` 또는 fallback 후보로 남긴다. fatal 판정은 validate에서 한다.

### 5-5. `normalizeConfigSnapshot(raw)`

역할:
- 문자열을 숫자/불리언/enum으로 변환
- 현재 `guardedStart()`의 clamp/fallback 의미를 보존
- `normalized`와 `derived`를 함께 만든다

반드시 보존할 현재 의미론:

1. `clanMode === 'TERRITORY'` 이면 `'SURNAME'`으로 normalize
2. herbivore species는 `trim().toUpperCase()` 후 `HERB_PROFILE` 없으면 fallback
3. fallback species는 우선 `REINDEER`, 없으면 `Object.keys(HERB_PROFILE)[0]`
4. `riverMax = mapType === 'TSL_XL_7200x4800' ? 8 : 6`
5. `riverCount`는 `0..riverMax`
6. `farmLandPct = 0.36 * (riverCount / riverMax)`
7. `mountainMax = mapType === 'TSL_XL_7200x4800' ? 8 : 6`
8. `altitudeEnabled === false` 면 `mountainCount = 0`
9. altitude ON이면 `mountainCount`는 `1..mountainMax`
10. `reproMinAgeTicks = CAL_TICKS_PER_YR * reproMinAgeYr`
11. `presetSeedEnabled && presetSeed > 0` 이면 resolved seed 사용
12. 그 외에는 map preset seed 사용 가능
13. `GAIA.earthTopo` 의미는 유지: `presetSeedEnabled && presetSeed == 12345 && TOPO_TEMPLATES[mapType]`
14. `TSL_LARGE`는 현재와 동일하게 width `3600 * 1.4`, height `3600`
15. preset이 있으면 preset `w/h`가 최종 우선

출력 형태 예시:

```js
{
  simulation: { ... },
  internal: { ... },
  derived: { ... },
  warnings: []
}
```

### 5-6. `validateNormalizedConfig(norm)`

역할:
- fatal / warning 구분
- fatal이면 시작 중단

fatal 예시:
- `mapType`가 비어 있고 preset 해석 불가
- normalize 후 `popSize`가 유효한 정수가 아님
- normalize 후 `predatorCount` 또는 `herbivoreCount`가 NaN
- `HERB_PROFILE`이 비어 fallback species 결정 불가

warning 예시:
- invalid herb species가 fallback으로 교정됨
- stale `TERRITORY`가 `SURNAME`으로 치환됨
- river/mountain 값이 clamp됨

반환 형식:

```js
{ ok: true, fatal: [], warnings: [] }
```

### 5-7. `reconcileConfigUI(norm)`

역할:
- normalize 결과를 DOM과 preview에 되반영
- 기존 `_sync*` 함수와 충돌 없이 동작

필수 반영 항목:
- fallback된 `cfg-herb-species` select 값 되쓰기
- clamp된 `cfg-river-count`, `cfg-mtn-count` 값 되쓰기
- `lbl-river-count`, `lbl-farm-pct`, `lbl-mtn-count` 표시 갱신
- `_syncClimateUI()`, `_syncAltUI()`, `_syncMarUI()`, `_syncMapDependentUI()` 중 필요한 재동기화 호출

금지:
- `CFG` 쓰기
- `GAIA` 쓰기
- localStorage 쓰기

### 5-8. `applyNormalizedConfigToCFG(norm)`

역할:
- `CFG.*` 쓰기를 이 함수 하나로 집중

필수 할당 대상:
- `CFG.popSize`
- `CFG.foodCount`
- `CFG.foodEnergy`
- `CFG.reproCost` (현행 700 유지)
- `CFG.reproMinAgeYr`
- `CFG.reproMinAgeTicks`
- `CFG.crisisRecoverSpeed`
- `CFG.predatorCount`
- `CFG.mutationRate`
- `CFG.predatorKillBonus`
- `CFG.mapType`
- `CFG.clanMode`
- `CFG.leaderMode`
- `CFG.pred2_threatLevel`
- `CFG.predatorSpecies`
- `CFG.herbivoreSpecies`
- `CFG.herbivoreCount`
- `CFG.herbivoreBonus`
- `CFG.herbivoreThreatLevel`
- `CFG.climateEnabled`
- `CFG.riverCount`
- `CFG.farmLandPct`
- `CFG.altitudeEnabled`
- `CFG.maritimeEnabled`
- `CFG.mountainCount`
- `CFG.worldWidth`
- `CFG.worldHeight`
- `CFG.presetSeedEnabled`
- `CFG.presetSeed`

주의:
- `refreshVisionCache()`는 이 단계 직후 또는 바로 다음 단계에서 1회 호출
- `disp-clan-mode` 같은 UI 쓰기는 여기서 하지 않는다

### 5-9. `persistConfigPreferences(norm)`

역할:
- start 이후 유지할 설정만 localStorage에 저장

1차 범위:
- `pred2_threatLevel`
- `leaderMode`
- 필요 시 `EVOSIM_CFG_TAB`

### 5-10. `prepareWorldgenFromNormalizedConfig(norm)`

역할:
- `GAIA.earthTopo`
- `GAIA.rand`
- `GAIA._seedBase`
- `GAIA._seedMtn`
- `GAIA._seedRiver`
- `GAIA._reseedForMountains`

을 현재 의미와 동일하게 준비한다.

주의:
- 여기서는 world reset/spawn을 하지 않는다.
- 순수하게 “seed/worldgen readiness”만 준비한다.

---

## 6. `guardedStart()` 치환 명세

## 6-1. 치환 원칙
현재 `guardedStart()` 초반부의 직접 DOM 읽기~seed 준비 구간을 아래 호출 체인으로 치환한다.

```js
ensureConfigNamespace();
const raw = collectRawConfigFromUI();
const norm = normalizeConfigSnapshot(raw);
const verdict = validateNormalizedConfig(norm);
if (!verdict.ok) throw new Error(verdict.fatal.join('\n') || 'Invalid configuration');
reconcileConfigUI(norm);
applyNormalizedConfigToCFG(norm);
persistConfigPreferences(norm);
prepareWorldgenFromNormalizedConfig(norm);
refreshVisionCache();
```

### 6-2. 유지해야 할 후속 구간
아래는 기존 `guardedStart()` 내부에서 그대로 유지한다.

- `setWorldGenState('generating', 'base-map', ...)` 이후의 reset 루틴
- 배열/queue/cache reset
- session reset
- clamor reset
- era reset
- camera reset
- staged GAIA worldgen 호출
- spawn/rebuild/E2I/ingame 진입

즉 이번 패치는 `guardedStart()`를 **설정/준비 파트만 경량화**하고, 월드 reset 이후 흐름은 손대지 않는다.

---

## 7. 구현 순서

### Step 1. 네임스페이스/기본 스캐폴딩 추가
- `EvoSim.config` namespace 도입
- `ensureConfigNamespace()` 추가
- `buildConfigDefaultsFromCFG()` 추가

### Step 2. 부트 hydrate 정리
- `LOCALSTORAGE_RESTORE_START`의 leader/threat restore를 `hydrateConfigUIFromDefaultsAndPrefs()`로 흡수
- 기존 동작이 깨지지 않도록 초기 실행 위치만 조정

### Step 3. collect 구현
- `collectRawConfigFromUI()` 추가
- 기존 `guardedStart()` 직독 코드를 여기로 이동

### Step 4. normalize 구현
- `normalizeConfigSnapshot(raw)` 추가
- species fallback / river / mountain / seed / preset 규칙 구현

### Step 5. validate 구현
- fatal/warning 분리
- invalid 시 `guardedStart()` 중단

### Step 6. reconcile 구현
- slider/select/label 동기화
- 기존 `_sync*`와 맞물리게 처리

### Step 7. apply / persist / worldgen 분리
- `applyNormalizedConfigToCFG()`
- `persistConfigPreferences()`
- `prepareWorldgenFromNormalizedConfig()`

### Step 8. `guardedStart()` 치환
- 기존 직접 할당 제거
- 새 파이프라인 호출로 대체

### Step 9. 회귀 검증
- 아래 10장 검증 체크리스트 수행

---

## 8. 금지사항

이번 패치에서 절대 깨지면 안 되는 것:

1. `leaderMode`, `predThreat` restore 동작
2. stale `TERRITORY` -> `SURNAME` normalize
3. invalid herb species fallback
4. altitude OFF 시 mountain 0 강제
5. farmLandPct 계산식
6. preset seed semantics
7. `earthTopo` 12345 특수 규칙
8. `TSL_LARGE` width 1.4 배 규칙
9. map preset width/height override
10. config overlay tab restore
11. 인게임 패널 collapse/state 동작
12. world reset 이후 staged worldgen 순서

---

## 9. 회귀 검증 체크리스트

### 9-1. 설정값 반영
- `cfg-pop`, `cfg-foodcount`, `cfg-food`, `cfg-repro-age`, `cfg-pred`, `cfg-mut`가 정상 반영된다.
- `cfg-pred-species`, `cfg-herb-species`, `cfg-herb-count`가 정상 반영된다.

### 9-2. species fallback
- 잘못된 herb species를 강제로 넣었을 때 경고 후 fallback된다.
- fallback된 값이 select에 되반영된다.

### 9-3. river/mountain 보정
- XL 맵에서 max가 8로 올라간다.
- 일반 맵에서 max가 6으로 유지된다.
- altitude OFF 시 mountainCount가 0이 된다.
- farm % 라벨이 현재 규칙대로 갱신된다.

### 9-4. preference persist/restore
- `leaderMode`를 바꾸고 재로딩했을 때 복원된다.
- `predThreat`를 바꾸고 재로딩했을 때 복원된다.
- config tab restore가 유지된다.

### 9-5. worldgen
- preset seed OFF/ON에서 기존과 같은 seed 동작을 보인다.
- seed 12345 + template 존재 시에만 `earthTopo`가 켜진다.

### 9-6. 세션/패널 비회귀
- config 패치 후 history/info/inspector panel이 무너지지 않는다.
- `__HS_UI` collapse/visible/localStorage 동작이 동일하다.

### 9-7. start flow
- Start 버튼으로 정상 worldgen이 시작된다.
- Resume flow는 기존처럼 동작한다.
- CONFIG -> INGAME, HOME -> CONFIG 이동이 깨지지 않는다.

---

## 10. 추천 구현 세부

### 10-1. 1차 패치에서는 `data-cfg-*` 도입을 미룬다
이유:
- 이번 목표는 수집 위치 집중화이지, DOM 스키마 전면 교체가 아니다.
- 먼저 함수 경계를 고정한 뒤, 2차 패치에서 binder로 전환하는 편이 안전하다.

### 10-2. 기존 `_syncMapDependentUI()`는 당장 폐기하지 않는다
이유:
- 현재 map/range/label 동기화가 이미 그 함수에 들어 있다.
- 1차 패치에서는 이를 `reconcileConfigUI()`에서 호출하는 방향으로 묶고, 이후 2차에서 분해한다.

### 10-3. defaults 정본화는 “완전 전환”이 아니라 “브리지”로 시작한다
즉,
- 먼저 `CFG` -> `defaults` 복사 브리지를 만들고
- hydrate/collect/normalize가 그 defaults를 참조하게 만든 다음
- 이후에 DOM 초기값 의존을 제거한다.

---

## 11. 구현 후 기대효과

1. 설정화면 패치가 `guardedStart()` 파손으로 이어지는 빈도가 크게 줄어든다.
2. 설정 카드 DOM을 바꿔도 수정 지점이 `collect/reconcile`로 좁혀진다.
3. 인게임 패널 상태와 설정화면 상태의 소유권이 분리된다.
4. 다음 단계인 `data-cfg-*` binder, config schema 문서화, 설정 카드 리디자인으로 넘어가기 쉬워진다.

---

## 12. 정책 필요 항목

현재 권장안은 아래다.

### 정책안 A. 1차 패치에서 `data-cfg-*` 미도입
- 장점: 회귀 위험 낮음
- 단점: DOM-코드 매핑 하드코딩이 잠시 남음

### 정책안 B. 1차 패치부터 `data-cfg-*` 일부 도입
- 장점: 이후 binder 전환 쉬움
- 단점: 이번 패치 범위가 불필요하게 커짐

**권장:** A안  
이번 패치의 핵심은 “시작 파이프라인 분해”이지 “DOM 스키마 혁신”이 아니다.

---

## 13. 다음 스텝

다음으로 바로 이어질 최적 산출물은 아래 둘 중 하나다.

1. **Codex 실행용 단일 프롬프트**  
   이번 작업명세를 바로 패치 실행 가능한 프롬프트로 압축

2. **패치 전 코드현황조사 보강본**  
   `guardedStart()` 치환 전후 대상 라인/대상 변수/대상 DOM을 표로 더 정밀하게 작성

권장 순서는 **Codex 실행용 프롬프트 작성**이다.
