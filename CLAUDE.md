# CLAUDE.md — EvoSim Quick Reference

> Last verified: 2026-03-04
> Canonical policy/docs source:
> - `docs/governance/MASTER_PROMPT.md`
> - `docs/ops/PROJECT_OPERATIONS.md`
> - `docs/spec/SPEC_OUTPUTS.md`

이 문서는 AI 실행자를 위한 **경량 온보딩 레퍼런스**다. 상세 정책은 위 canonical 문서를 우선한다.

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
