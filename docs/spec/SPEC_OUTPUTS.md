# EvoSim Output Specification

## 1) 공통 규칙
- 정책 판단 필요 시 대안 2개 이상 + 권장안 제시.
- 모든 산출물 말미에 "다음 스텝" 제안 포함.
- 중요도(경미/중간/중요) 판정은 작성 단계에서 1차 제안한다.

## 2) 산출물 유형별 필수 필드

### 기획서
- 목표 과제
- 해결 전략
- UX/내러티브 정합성
- 정책 필요 항목

### 작업로드맵
- 단계(Phase/Step)
- 선행 조사 항목
- 실행 순서

### 코드현황조사
- 관련 BUILDER_BLOCK 목록
- 현재 구현 상태
- 변경 대상 함수/변수

### 작업명세
- 파일/함수/변수 단위 지시
- 수식/의사코드
- 주석/블록 추가 지점

### 프롬프트
- [구분] / 중요도
- [목표]
- [변경 범위]
- [금지사항]
- [구현 지침]
- [FINAL 지침] (필수)
  1) `index.html`(또는 단일 메인 HTML) 변경 반영
  2) 최종 적용 완료본을 `docs/releases/EvoSim_latest.html`로 동기화
  3) `docs/releases/archive/EvoSim_<version>_<patch>.html` 생성
  4) `docs/releases/EvoSim_CHANGELOG.txt` 최종 항목 확정(중간/중요인 경우에만 필수)
  5) 위 필수 항목 완료 후에만 PR Ready 처리

### 실행보고
- 변경내용(10문장 이내)
- 프롬프트와 다르게 변경한 사항(4문장 이내, 있으면 작성)
- 주석변경사항(2문장 이내, 있으면 작성)
- 권고사항(1문장 이내, 있으면 작성)

#### 선택 메타필드(권장)
- 대상 파일명
- 완성 파일명
- 프롬프트 적용 요약(프롬프트별, 각 10문장 이내)
- 추가 검증 로그/스크린샷 경로

### 의견서
- 수정필수/중대/수정권고/경미 분류
- 근거
- 대안

### CHANGELOG
- 중간 이상 패치 필수
- `docs/releases/EvoSim_CHANGELOG.txt`에 누적

## 3) 중요도 라우팅 통합표
- 경미: 기록 생략 가능 (필요 시 인라인 주석)
- 중간: CHANGELOG 반영
- 중요: ARCHITECTURE_OVERVIEW + PATCH RECORD + CHANGELOG

## 4) PR 동반 산출물 최소 세트
- 코드 변경 diff
- 실행보고
- changelog 항목(중간/중요)
- FINAL 지침 완료 증거(latest/archive/changelog 반영)
