# CLAUDE.md — EvoSim Quick Reference

> Last verified: 2026-03-04
> Current version: **27.10** (terrain footing-load precise-alt)
> Canonical policy/docs source:
> - `docs/governance/MASTER_PROMPT.md`
> - `docs/ops/PROJECT_OPERATIONS.md`
> - `docs/spec/SPEC_OUTPUTS.md`

이 문서는 AI 실행자를 위한 **실무 레퍼런스(요약+실행체크)**다. 상세 정책의 단일 소스는 위 canonical 문서다.
빠른 체크리스트는 유지하되, 운영 누락이 잦은 항목(버전 타이틀 동기화/중요 패치 기록 레이어/아카이브 동기화)을 함께 다룬다.

---

## 1) Project snapshot

- EvoSim은 **단일 HTML 기반**(주 실행: `index.html`, 760KB / 18,500+ 줄)의 시뮬레이터다.
- 배포 최신본은 `docs/releases/EvoSim_latest.html`.
- 이력 스냅샷은 `docs/releases/archive/` (10개 버전, 27.03 → 27.10).
- 빌드 도구: Vite + TypeScript (`npm run dev` → localhost:3000).
- 외부 의존성: `@google/genai` (Google Gemini API — `GEMINI_API_KEY` 환경변수 필요).

**Repository layout:**
```
index.html                        ← Main app (single source of truth for code)
docs/
  releases/
    EvoSim_latest.html            ← Canonical deployment snapshot (sync with index.html)
    EvoSim_CHANGELOG.txt          ← Patch log (newest first)
    archive/                      ← Immutable version snapshots
  governance/MASTER_PROMPT.md     ← Master policy & AI persona
  ops/PROJECT_OPERATIONS.md       ← GitHub workflow & branching
  ops/RELEASE_DELIVERY_CHECKLIST.md
  ai/AI_HANDOFF_PROTOCOL.md
  spec/SPEC_OUTPUTS.md            ← Output format specifications
metadata.json                     ← Project metadata
package.json / vite.config.ts / tsconfig.json
```

---

## 2) Codebase architecture

`index.html` is a monolithic file organized by **211 `BUILDER_BLOCK` markers**:

```
BUILDER_BLOCK: STYLES_START/END         ← CSS root variables
BUILDER_BLOCK: CSS_START/END            ← All CSS declarations
BUILDER_BLOCK: UI_DOM_START/END         ← HTML UI layer
BUILDER_BLOCK: ARCHITECTURE_OVERVIEW_START/END  ← Embedded design doc
BUILDER_BLOCK: JS_TOP_START/END         ← Imports & globals
BUILDER_BLOCK: JS_BOOTSTRAP_CORE_START/END
BUILDER_BLOCK: WORLDGEN_UI_INIT_START/END
BUILDER_BLOCK: CONFIG_TABS_START/END
... (205+ more sections)
```

**Key subsystems:**

| Subsystem | Description |
|-----------|-------------|
| Worldgen Pipeline | Mountains → maritime SOT → altitude → hydrology → climate → seasons |
| Agent/Neural System | GRU neural nets, 27 NN input slots, genetics, evolution |
| Tribe/Clan System | Surnames, genealogy, warfare, cooperation |
| Agricultural System | Farm growth, dT multipliers, temperature-dependent yields |
| Climate System | Seasons, temperature, humidity, hemisphere/lag effects |
| Terrain System | GAIA footing-load, 4-triangle slope/roughness, coastal distance |
| Rendering System | Canvas 2D (`simCanvas`, `brainCanvas`, `popGraphCanvas`) |
| Sensor System | Vision, audio (4-ch), olfaction, proprioception, interoception, social |
| Time/Event System | Tick-based simulation, historical event logging |

**NN sensor slots (v27.10):**
- Slot 15 = `FOOTING_LOAD` (접지·지지부하 — continuous precise altitude via `altitude01AtPrecise(wx, wy)`)
- Roughness path (`KR_ROUGH_ALT`, `GAIA.altRough01`) **removed** in 27.10

**Canvas elements:** `simCanvas` (main), `brainCanvas` (neural viz), `popGraphCanvas` (population graph).

**Edit principle:** Prefer BUILDER_BLOCK unit edits. Do not scatter changes across blocks without cause.

---

## 3) Development environment

```bash
npm install          # Install dependencies
npm run dev          # Vite dev server → http://localhost:3000
npm run build        # Production build
npm run preview      # Preview production build
```

Environment variable required:
```
GEMINI_API_KEY=<your-key>    # Injected by Vite at build/dev time
```

TypeScript: `tsconfig.json` — target ES2022, moduleResolution bundler, path alias `@/` → repo root.

---

## 4) Branch interpretation (핵심)

- 표준 규칙: `release/<version>-<task>`, `fix/<task>`, `docs/<task>`, `feature/<task>`.
- 실행 환경 세션 브랜치(`claude/*`, `codex/*`, `work/*`)는 **실행 레이어**이며 표준 규칙과 공존한다.
- 버전 증가 작업은 최종 PR/커밋/브랜치 문맥에서 `release/<version>-<task>` 식별이 보여야 한다.

**Git strategy:** Session/operative branches → merge to `main` via PR. One version = one complete workflow.
**Commit granularity:** One prompt = one commit. One version = one PR.

---

## 5) Output sync (핵심) — 3-way version title lock

코드 동작 변경 후 아래 3가지가 **동시에** 일치해야 한다 (Version Title Lock Design A):

| Artifact | Location | Must match |
|----------|----------|------------|
| HTML `<title>` | `index.html` + `docs/releases/EvoSim_latest.html` | `EvoSim <ver> <patch>` |
| CHANGELOG header | `docs/releases/EvoSim_CHANGELOG.txt` | same version string |
| Archive filename | `docs/releases/archive/EvoSim_<ver>_<patch>_FINAL.html` | same version string |

- 코드 동작 변경 시: `index.html` ↔ `docs/releases/EvoSim_latest.html` 동기화.
- 코드 동작 변경 시: archive 스냅샷 생성(중요도와 무관).
- CHANGELOG는 중간/중요 패치만 필수, 경미 패치는 생략 가능.

**Archive naming convention:**
```
EvoSim_<version>_<patch-description>_FINAL.html        ← release milestone
EvoSim_<version>_<patch-description>_FINAL_fix1.html   ← post-FINAL correction
EvoSim_<version>_<patch-description>_FINAL_fix2.html   ← second correction
```

---

## 6) CHANGELOG short format

파일: `docs/releases/EvoSim_CHANGELOG.txt` (newest first)

```
[EvoSim <version> — <patch-name>]
Change: <1–4 lines describing what changed>
Why:    <1 line — motivation>
Impact: <1 line — scope of effect>
Verify: <optional 1 line — how to confirm>
```

---

## 7) Execution report format (4 sections)

`docs/spec/SPEC_OUTPUTS.md`의 실행보고 4섹션을 준수:

1. **변경내용** (≤10 sentences): What changed
2. **프롬프트와 다르게 변경한 사항** (≤4 sentences): Deviations from prompt
3. **주석변경사항** (≤2 sentences): Documentation/comment updates
4. **권고사항** (≤1 sentence): Recommended next step

---

## 8) Path rule

- 프롬프트와 산출물에는 저장소 **상대경로**만 사용한다.
- 절대경로(`/mnt/data`, `/tmp`, `/home/user/...` 등) 사용 **금지**.

---

## 9) Practical checklist

**Before any code change:**
- [ ] BUILDER_BLOCK 단위로 편집 범위를 특정했는가?
- [ ] 변경 범위가 문서/코드 정책과 일치하는가?

**After any code change:**
- [ ] 코드 동작 변경이면 `index.html` ↔ `docs/releases/EvoSim_latest.html` 동기화를 완료했는가?
- [ ] archive 스냅샷을 올바른 명명 규칙으로 생성했는가?
- [ ] CHANGELOG 형식(단문 템플릿)과 정렬(newest first)을 지켰는가?
- [ ] 실행보고 4섹션을 작성했는가?
- [ ] `target_version`과 타이틀/CHANGELOG/archive 버전이 **3-way 일치**하는가?
- [ ] NN I/O 계약 변경 시 `ARCHITECTURE_OVERVIEW` 블록을 갱신했는가?

**Merge gate (6-point):**
1. latest sync complete
2. archive snapshot created with correct filename
3. CHANGELOG updated (mid/critical only)
4. Execution report 4-section written
5. `release/*` branch used for version-bump PRs
6. 3-way version match verified

---

## 10) Team terms (고정 용어)

- **기획팀(planning)**: 작업명세/품질 기준/리스크 제기 담당
- **개발팀(executing)**: 패치 적용/브랜치·커밋·PR/실행보고 담당
- 본 문서의 `개발팀` 표기는 Claude Code/Codex 실행 주체를 의미한다.

---

## 11) Important patch note (기록 레이어)

- 중요 패치는 CHANGELOG 외에 코드 내 `PATCH RECORD` 또는 `ARCHITECTURE_OVERVIEW` 갱신을 권장한다.
- 구조 변경(파이프라인/스키마/의존관계/NN I/O 계약)은 `ARCHITECTURE_OVERVIEW` 우선 반영한다.
- 중요도 라우팅: minor(CHANGELOG 생략 가능) / mid(CHANGELOG 필수) / critical(CHANGELOG + ARCHITECTURE_OVERVIEW).

---

## 12) Current state (v27.29)

| Field | Value |
|-------|-------|
| Version | 27.29 |
| Title | herbivore pack-to-herd rename |
| index.html | Synced ✓ |
| EvoSim_latest.html | Synced ✓ |
| Archive | `EvoSim_27.29_herbivore-pack-to-herd-rename_FINAL.html` ✓ |
| CHANGELOG | Updated ✓ |
| Key change | 초식동물 시스템 pack→herd 전면 리네임. 전역상수/함수명/클래스필드/프로필필드/지역변수/주석·로그 포함. 포식자 pack 보존. 동작변경 없음(순수 rename). |

**Recent version lineage (newest first):**
- 27.29 — herbivore pack-to-herd rename
- 27.28 — builder-block GAP finalize
- 27.27 — cfg-defaults consolidate
- 27.26 — builder block refactor 3D (HS_ 완전 소멸)
- 27.25 — builder block refactor 3C
- 27.24 — builder block refactor 3B-3
- 27.21 — builder block refactor Phase 3-A
- 27.20 — Hard module base FINAL
- 27.11 — boot-init strip-yield finalize-reorder
- 27.10 fix1 — initAgro scope fix (critical)
- 27.10 — terrain footing-load precise-alt
- 27.09 — agent TEMP seasoned input FINAL
- 27.08 — farm temp dT mul + AGRO hydrology (+ fix1, fix2)
