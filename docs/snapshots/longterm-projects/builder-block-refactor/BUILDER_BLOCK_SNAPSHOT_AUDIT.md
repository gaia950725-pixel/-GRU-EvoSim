# BUILDER_BLOCK Snapshot Audit (EvoSim)

**점검일자:** 2026-03-05  
**대상 스냅샷:** `BUILDER_BLOCK_SNAPSHOT.md`  
**대조 기준:** 루트 `index.html`의 `BUILDER_BLOCK:*_(START|END)` 마커

## 점검 결과 요약

- 27.20 Hard module base FINAL 기준 스캔 결과: **START 118 / END 118 / 고유 블록 118 / 이름별 불일치 0**.
- releases 동기화 완료: `docs/releases/EvoSim_latest.html`, `docs/releases/archive/EvoSim_27.20_fix1.html`가 `index.html`과 동일 복사.

## 자동 산출 근거 (복붙형 커맨드)

```bash
sha256sum index.html docs/releases/EvoSim_latest.html docs/releases/archive/EvoSim_27.20_fix1.html

node -e "const fs=require('fs');const s=fs.readFileSync('index.html','utf8');const sc=new Map(),ec=new Map();let depth=0,maxD=0,neg=false;const reTok=/BUILDER_BLOCK:\s*([^\n]*?)_(START|END)\b/g;let m;while((m=reTok.exec(s))){const name=m[1].trim(),kind=m[2];if(kind==='START'){sc.set(name,(sc.get(name)||0)+1);depth++;if(depth>maxD)maxD=depth;}else{ec.set(name,(ec.get(name)||0)+1);depth--;if(depth<0)neg=true;}}const startTotal=[...sc.values()].reduce((a,b)=>a+b,0);const endTotal=[...ec.values()].reduce((a,b)=>a+b,0);const mismatch=[];for(const [k,v] of sc){const ev=ec.get(k)||0;if(ev!==v)mismatch.push(k+':'+v+'/'+ev);}for(const [k,v] of ec){if(!sc.has(k))mismatch.push(k+':0/'+v);}console.log(JSON.stringify({startTotal,endTotal,uniq:sc.size,maxDepth:maxD,depthEnd:depth,negDepth:neg,mismatch},null,2));"
```

## 체크리스트

- [x] START/END 총수 일치 (118/118)
- [x] depthEnd=0, negDepth=false
- [x] 이름별 START/END 불일치 0개
- [x] `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_LEGACY_STEP1_*` 1쌍
- [x] `HS_FITTER_P1_3_TOGGLERIGHT_DIRTY_*` (Rev31) 1쌍
- [x] `AGENT_DRAW_TAIL`, `CANVAS_CONTEXT_INIT`, `UI_DRAWERS_STEP1_INIT` 존재 확인
