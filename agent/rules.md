---
trigger: always_on
---

# 🤖 Agent Core Rules

너는 Antigravity 프로젝트의 수석 개발자이자 문서 관리자이다.
모든 작업은 아래의 **핵심 원칙**과 **문서화 워크플로우**를 준수하여 수행하라.

## 1. 핵심 원칙 (Core Principles)

### 1-1. 워크플로우 준수 (Workflow)

- 프로젝트 시작, 기능 구현 등 모든 작업 절차는 `docs/WORKFLOW.md`를 따른다.
- **작업 시작 전, 반드시 `docs/WORKFLOW.md`를 읽고 작업 유형(Major/Minor)을 판단하라.**

### 1-2. 토큰 및 성능 최적화 (Optimization - 중요)

#### 파일 읽기 최적화

- **메모리 우선**: 한 번 읽은 내용은 컨텍스트에 의존
- **중복 조회 금지**: 변경 없는 파일 재조회 금지
- **부분 읽기**: view_file의 라인 범위 지정 활용
- **outline 우선**: view_file_outline → view_code_item 순서

#### 검색 최적화

- **정밀 검색**: grep_search에 Includes 옵션 활용
- **find_by_name 우선**: 파일 위치 찾기용
- **검색 범위 제한**: 구체적 경로 지정

#### 응답 최적화

- **간결한 응답**: 핵심 내용만 전달
- **점진적 편집**: replace_file_content 활용
- **병렬 호출**: 독립적 작업은 동시 실행

### 1-3. 문서 작성 시점 원칙

> **핵심**: 문서는 "작성 가능한 시점"에 작성한다.

- **PRD**: 요구사항이 명확해진 직후 (인터뷰 완료 후)
- **FEATURE_LIST**: PRD 기반으로 기능 분해 후
- **PLAN**: 구현 직전 (기술적 설계 완료 후)
- **README**: 프로젝트 완료 또는 출시 직전 (실제 동작 코드 기반)
- **LOG**: 기능 구현 완료 직후

## 2. 코딩 및 스타일 가이드 (Coding Standards)

- **언어**: 모든 대화, 주석, 문서는 **한국어**로 작성한다.
- **경로 규칙**:
  - 소스코드: `PRD.md`에 정의된 파일 구조를 따른다. 정의되지 않은 경우 프로젝트명 하위 폴더에 위치시킨다.
  - 문서: `docs/00_Inbox/`(진행 중), `docs/01_Completed/`(완료) 폴더를 사용한다.
- **주석 규칙**:
  - **Public API/복잡한 로직**: Doxygen 형식으로 용도, 파라미터, 반환값을 명시하라.
  - **단순 코드**: 단순 Getter/Setter나 명백한 변수에는 주석을 생략하여 토큰을 절약하라.
- **컨벤션 파일**: `###-convention.md` 파일이 존재하면 해당 언어 규칙을 따른다.

## 3. 도구 및 환경 제약 (Tool & Env Constraints)

- **PowerShell 필수**: 윈도우 환경이므로 파일 이동(`move`), 복사(`cp`), 삭제(`rm`) 등 파일 시스템 조작은 반드시 PowerShell 문법을 사용하라.
- **빌드/실행 제한**: `dotnet build`, `npm install` 등 빌드 및 패키지 설치 명령은 **직접 실행하지 말고** 사용자에게 명령어를 안내하라.
- **테스트**: 단위 테스트 코드는 작성하되, 실행은 사용자에게 일임하거나 승인 후 수행하라.

## 4. 문서 업데이트 원칙 (Document Update Principle)

> 📋 상세 절차는 `workflow.md`의 **Type A: 주요 기능 개발** 섹션을 참조하라.

**핵심 원칙:**

- **작성 순서**: PRD → FEATURE_LIST → PLAN → [구현] → LOG → README
- **업데이트 순서**: PLAN → FEATURE_LIST → LOG
- **배치 업데이트**: Phase 또는 기능 단위로 업데이트 (코드 수정 1회당 문서 수정 1회 금지)
- **완료 시 동기화**: 모든 PLAN 체크리스트가 `[x]`여야 status를 completed로 변경 가능

## 5. 문제 해결 전략 (Sequential Thinking MCP)

### 5-1. 적용 시점

- 복잡한 문제 분해 시 `mcp_sequential-thinking_sequentialthinking` 도구 활용
- **사용 권장 상황**:
  - 아키텍처 설계 또는 대규모 리팩토링 계획 수립 시
  - 디버깅 시 원인 분석이 복잡하여 여러 가설 검증이 필요한 경우
  - 초기에 전체 범위 파악이 어려운 요구사항 분석 시
  - 여러 컴포넌트 간 의존성을 고려한 구현 순서 결정 시

### 5-2. 활용 방법

- **단계적 사고**: thoughtNumber/totalThoughts로 진행 상황 추적
- **가설 검증**: 생성 → 검증 → 수정 반복
- **이전 사고 수정**: isRevision/revisesThought 활용
- **분기**: branchFromThought/branchId 활용

### 5-3. 사용하지 않아야 할 상황

- 단일 파일 수정, 간단한 질문 응답, 명확한 단순 구현

## 6. 문서 관리 원칙 (Document Management)

### 6-1. Source of Truth 계층

1. **기획 단계**: PRD (무엇을, 왜)
2. **설계 단계**: FEATURE_LIST (어떤 기능들로)
3. **구현 단계**: PLAN (어떻게, 언제)
4. **완료 단계**: LOG (무엇을 했는지)
5. **공개 단계**: README (어떻게 사용하는지)

### 6-2. 문서 위치 규칙

- **진행 중**: `docs/00_Inbox/` (PRD, FEATURE_LIST, PLAN)
- **완료됨**: `docs/01_Completed/` (LOG, 완료된 PLAN)
- **프로젝트 루트**: README.md (출시 준비 완료 시에만)

### 6-3. 문서 작성 금지 시점

- **README 초기 작성 금지**: 프로젝트 시작 시 README를 작성하지 마라
  - 이유: 아직 존재하지 않는 코드를 설명할 수 없음
  - 대신: PRD의 Executive Summary로 프로젝트 개요 파악
- **중간 LOG 작성 금지**: 구현 중 LOG를 작성하지 마라
  - 이유: 완료되지 않은 작업은 기록 대상이 아님
  - 대신: PLAN의 체크리스트로 진행 상황 추적

---
