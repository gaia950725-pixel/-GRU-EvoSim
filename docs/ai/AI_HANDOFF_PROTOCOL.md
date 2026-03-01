# AI Handoff Protocol (ChatGPT / Gemini / Codex / Claude)

## Fixed header to include in task prompts
1. 저장소 상대경로만 사용한다.
2. canonical 결과물은 `docs/releases/EvoSim_latest.html`에 반영한다.
3. 버전 스냅샷은 `docs/releases/archive/`에 생성한다.
4. 중간/중요 패치는 `docs/releases/EvoSim_CHANGELOG.txt`를 갱신한다.
5. 동일 버전 작업은 동일 브랜치에서 누적한다.
6. 머지 전 latest/archive/changelog 동기화를 점검한다.

## Execution handoff package
- 입력: 작업명세 + 프롬프트 세트 + 기준 버전 + 기준 브랜치
- 출력: 커밋 목록 + 변경 파일 목록 + 실행보고

## Diff-first principle
- 전체 파일 재전송보다 변경 블록(diff) 중심으로 전달한다.
- BUILDER_BLOCK 단위 편집을 우선한다.
