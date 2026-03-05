# BUILDER_BLOCK Snapshot — EvoSim index.html

**최초 작성일:** 2026-03-04  
**최종 갱신일:** 2026-03-05 (v27.20_fix1 라인번호 재스캔)  
**대상 파일:** `index.html`  
**기준 버전:** `v27.20_fix1` (`docs/releases/archive/EvoSim_27.20_fix1.html`)  
**총 BUILDER_BLOCK 마커 수:** 236개 (START/END 118쌍)

---

## 1) 재스캔 요약

- 스캔 결과: **START 118 / END 118 / 고유 블록 118 / 이름별 불일치 0**
- 최대 중첩 깊이: **4**
- 현재 `index.html`, `docs/releases/EvoSim_latest.html`, `docs/releases/archive/EvoSim_27.20_fix1.html`는 동일 라인 수(18,534줄) 기준으로 동기화됨.
- 기존 문서에 남아 있던 `v27.11` 기준 라인번호/카운트(119쌍)는 본 스냅샷에서 폐기.

---

## 2) 장기 리팩토링용 핵심 앵커 블록 (v27.20_fix1)

| 구분 | 블록명 | 라인 범위 | 규모(약) | 비고 |
|---|---|---:|---:|---|
| 대형 | `UI_NAV_ROUTING` | 9726–11471 | 1745줄 | 중기 분할 1순위 |
| 대형 | `HS_COLLISION_LOOP` | 13729–14740 | 1011줄 | 중기 분할 1순위 |
| 대형 | `AGENT_GRID_CORE` | 8092–9077 | 985줄 | 중기 분할 1순위 |
| 대형 | `HS_PHASE1_HELPERS` | 16214–17107 | 893줄 | 하위 `SIM_LOOP` 보유 |
| 핵심 | `SPAWN_AND_LOG_CORE` | 13238–13613 | 375줄 | 로그/스폰 코어 |
| 핵심 | `UI_STARTUP_CONTROLS_INIT` | 9477–9722 | 245줄 | GAP-B 해소 블록 |
| 핵심 | `UI_DRAWERS_STEP1_INIT` | 17938–18174 | 236줄 | GAP-H 해소 블록 |
| 핵심 | `PREDATOR_CLASS_CORE` | 11716–12166 | 450줄 | GAP-C 해소 블록 |
| 핵심 | `HERBIVORE_CLASS_CORE` | 12167–12407 | 240줄 | GAP-C 해소 블록 |

---

## 3) 미해소 GAP 현황 (v27.20_fix1)

| GAP | 라인 범위 | 규모(약) | 상태 | 권장 블록명 |
|---|---:|---:|---|---|
| GAP-A | 8035–8091 | 57줄 | 미해소 | `TS_SOCIAL_VOICE_GLOBALS` |
| GAP-D | 13614–13728 | 115줄 | 미해소 | `FOOD_GRID_CORE` |
| GAP-E | 14741–14842 | 102줄 | 미해소 | `BRAIN_CANVAS_DRAW` |
| GAP-F | 15200–15349 | 150줄 | 미해소 | `UI_SYSTEM_PANELS_UPDATE` |
| GAP-G | 15804–15889 | 86줄 | 미해소 | `FOOD_PHASE0_SIM_UTILS` |
| GAP-I | 18413–18469 | 57줄 | 미해소 | `UI_CHIP_TABS_PNREV_INIT` |

> 참고: GAP-B/C/H는 27.20 계열에서 해소 완료.

---

## 4) 중복명 이슈 상태

- 과거 중복 이슈였던 `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY`는 다음과 같이 분리되어 충돌이 해소된 상태.
  - `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_LEGACY_STEP1`: 18097–18102
  - `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY`: 18255–18260

---

## 5) 재생성 커맨드(재현용)

```bash
node -e "const fs=require('fs');const s=fs.readFileSync('docs/releases/archive/EvoSim_27.20_fix1.html','utf8');const re=/BUILDER_BLOCK:\\s*([^\\n]*?)_(START|END)\\b/g;let m,depth=0,maxDepth=0,neg=false;const sc=new Map(),ec=new Map();while((m=re.exec(s))){const n=m[1].trim(),k=m[2];if(k==='START'){sc.set(n,(sc.get(n)||0)+1);depth++;if(depth>maxDepth)maxDepth=depth;}else{ec.set(n,(ec.get(n)||0)+1);depth--;if(depth<0)neg=true;}}const mismatch=[];for(const [k,v] of sc){const ev=ec.get(k)||0;if(ev!==v)mismatch.push(k+':'+v+'/'+ev);}for(const [k,v] of ec){if(!sc.has(k))mismatch.push(k+':0/'+v);}const startTotal=[...sc.values()].reduce((a,b)=>a+b,0);const endTotal=[...ec.values()].reduce((a,b)=>a+b,0);console.log(JSON.stringify({startTotal,endTotal,uniq:sc.size,maxDepth,depthEnd:depth,negDepth:neg,mismatch},null,2));"
```

