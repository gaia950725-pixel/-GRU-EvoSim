# CLAUDE.md — EvoSim Quick Reference

> Last verified: 2026-03-04
> Canonical policy/docs source:
> - `docs/governance/MASTER_PROMPT.md`
> - `docs/ops/PROJECT_OPERATIONS.md`
> - `docs/spec/SPEC_OUTPUTS.md`

이 문서는 AI 실행자를 위한 **실무 레퍼런스(요약+실행체크)**다. 상세 정책의 단일 소스는 위 canonical 문서다.
빠른 체크리스트는 유지하되, 운영 누락이 잦은 항목(버전 타이틀 동기화/중요 패치 기록 레이어/아카이브 동기화)을 함께 다룬다.

## 1) Project snapshot
- EvoSim은 단일 HTML 기반(주 실행: `index.html`)의 시뮬레이터다.
- 배포 최신본은 `docs/releases/EvoSim_latest.html`.
- 이력 스냅샷은 `docs/releases/archive/`.

## 2) Branch interpretation (핵심)
- 표준 규칙: `release/<version>-<task>`, `fix/<task>`, `docs/<task>`, `feature/<task>`.
- 실행 환경 세션 브랜치(`claude/*`, `codex/*`, `work/*`)는 **실행 레이어**이며 표준 규칙과 공존한다.
- 버전 증가 작업은 최종 PR/커밋/브랜치 문맥에서 `release/<version>-<task>` 식별이 보여야 한다.

## 3) Output sync (핵심)
- 코드 동작 변경 시: `index.html` ↔ `docs/releases/EvoSim_latest.html` 동기화.
- 코드 동작 변경 시: archive 스냅샷 생성(중요도와 무관).
- CHANGELOG는 중간/중요 패치만 필수, 경미 패치는 생략 가능.

## 4) CHANGELOG short format
파일: `docs/releases/EvoSim_CHANGELOG.txt`
- `Change`: 1~4줄
- `Why`: 1줄
- `Impact`: 1줄
- `Verify`: 0~1줄(선택)

## 5) Execution report format
`docs/spec/SPEC_OUTPUTS.md`의 실행보고 4섹션을 준수:
1) 변경내용
2) 프롬프트와 다르게 변경한 사항
3) 주석변경사항
4) 권고사항

## 6) Path rule
- 프롬프트와 산출물에는 저장소 **상대경로**만 사용한다.
- 절대경로(`/mnt/data`, `/tmp` 등) 사용 금지.

## 7) Practical checklist
- [ ] 변경 범위가 문서/코드 정책과 일치하는가?
- [ ] 코드 동작 변경이면 latest + archive 동기화를 완료했는가?
- [ ] CHANGELOG 형식(단문 템플릿)과 정렬(newest first)을 지켰는가?
- [ ] 실행보고 4섹션을 작성했는가?
- [ ] `target_version`와 타이틀/CHANGELOG/archive 버전이 일치하는가?


## 8) Team terms (고정 용어)
- 기획팀(planning): 작업명세/품질 기준/리스크 제기 담당
- 개발팀(executing): 패치 적용/브랜치·커밋·PR/실행보고 담당
- 본 문서의 `개발팀` 표기는 Claude Code/Codex 실행 주체를 의미한다.

## 9) Important patch note (기록 레이어)
- 중요 패치는 CHANGELOG 외에 코드 내 `PATCH RECORD` 또는 `ARCHITECTURE_OVERVIEW` 갱신을 권장한다.
- 구조 변경(파이프라인/스키마/의존관계/NN I/O 계약)은 `ARCHITECTURE_OVERVIEW` 우선 반영한다.
