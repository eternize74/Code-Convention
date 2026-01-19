---
type: #type/plan
status: #status/in-progress
created: {{YYYYMMDD_HHMM}}
feature_id: {{FEATURE_ID}}
related_feature_list: [[FEATURE_LIST_{{프로젝트명}}]]
---

# 📋 PLAN: {{기능명}}

## 1. 목표 (Goal)

- **사용자 요청:** (사용자의 원본 프롬프트 요약)
- **핵심 목표:** (이번 작업으로 달성해야 할 기술적 목표 1줄 요약)

## 2. 구현 계획 (Implementation Steps)

각 항목은 **세부 기능 단위**로 작성한다.

- [ ] (세부 기능 1) 예: 계산기 HTML 구조 작성
- [ ] (세부 기능 2) 예: 다크 모드 스타일시트 작성
- [ ] (세부 기능 3) 예: 숫자 입력 로직 구현
- [ ] (세부 기능 4) 예: 브라우저 테스트 및 검증

## 3. 구현 내용 (Implementation Plan)

구현할 내용을 세부적으로 작성한다.
동작 방식 및 흐름을 설명한다.
변수는 `{{변수명}} : {{사용용도}}` 형식으로 작성한다.
함수 구현은 함수명, 입력, 출력, 설명 순서로 작성한다.
주석은 `/** @brief 설명 */` 형식으로 작성한다.
사용되거나 추가 설치된 모듈이나 패키지의 용도 및 설치 방법, 버전, 링크, 라이선스등을 명시한다.

### 함수 구현 예시

```javascript
function add(a, b) {
  return a + b;
}
```

### 주석 예시

```javascript
/**
 * @brief 함수 설명
 * @param arg1 매개변수 설명
 * @return 반환값 설명
 */
```

## 4. 영향 범위 (Impact Analysis)

- **수정/생성 예상 파일:**
  - `path/to/file.ext` - 설명
- **주의 사항:** (사이드 이펙트 가능성)

## 5. 변경 요청 이력 (Change Requests)

(변경 요청 시 아래 형식으로 추가)

### CR-XXX: 변경 제목 (YYYY-MM-DD)

- **요청 내용**:
- **변경 계획**:
- **수정 파일**:
- **상태**: [ ] 대기 / [/] 진행 중 / [x] 완료

## 6. 관련 문서 (Related Documents)

- 개발 로그: [[LOG_{{YYYYMMDD_HHMM}}_{{기능명}}]]