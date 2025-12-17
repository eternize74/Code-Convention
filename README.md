# 📚 코드 컨벤션 가이드 (Code Conventions Guide)



이 디렉토리는 다양한 프로그래밍 언어에 대한 코드 컨벤션 문서를 포함하고 있습니다.  

프로젝트 전반에서 일관된 코드 스타일을 유지하기 위해 해당 언어의 컨벤션을 참조하세요.



---



## 📋 지원 언어



| 언어 | 문서 | 기반 가이드라인 |
|------|------|-----------------|
| C# | [csharp-convention.md](./CodeConvention/csharp-convention.md) | Microsoft 공식 가이드라인 |
| Python | [python-convention.md](./CodeConvention/python-convention.md) | PEP 8 스타일 가이드 |
| Java | [java-convention.md](./CodeConvention/java-convention.md) | Google Java Style Guide |
| JavaScript | [javascript-convention.md](./CodeConvention/javascript-convention.md) | Airbnb Style Guide |
| TypeScript | [typescript-convention.md](./CodeConvention/typescript-convention.md) | TypeScript 공식 핸드북 |



---



## 🎯 공통 원칙



모든 언어에 적용되는 공통적인 코딩 원칙입니다:



### 1. 가독성 우선



```

코드는 쓰는 것보다 읽는 횟수가 훨씬 많습니다.

항상 다른 개발자(또는 미래의 자신)가 읽기 쉬운 코드를 작성하세요.

```



### 2. 일관성 유지



- 프로젝트 전체에서 동일한 스타일 유지
- 기존 코드베이스의 스타일을 따름
- 팀 내 합의된 규칙 준수



### 3. 명확한 명명



- 의미 있고 설명적인 이름 사용
- 축약어 사용 최소화
- 이름만으로 용도를 알 수 있게



### 4. 주석은 "왜"를 설명



```

// ❌ Bad: 무엇을 하는지 설명 (코드를 보면 알 수 있음)

i = i + 1;  // i를 1 증가시킴



// ✅ Good: 왜 하는지 설명

i = i + 1;  // 경계값 보정 (API는 0-based, DB는 1-based)

```



### 5. 단순함 추구



- 복잡한 로직은 작은 함수로 분리
- 중첩은 최대 3-4단계까지
- 한 함수에 한 가지 책임



---



## 🛠️ 린터 및 포맷터 설정



각 언어별 권장 도구입니다:



| 언어 | 린터 | 포맷터 |
|------|------|--------|
| C# | Roslyn Analyzers | dotnet format |
| Python | Flake8, Pylint | Black, autopep8 |
| Java | Checkstyle, PMD | google-java-format |
| JavaScript | ESLint | Prettier |
| TypeScript | ESLint + TypeScript | Prettier |



### 설정 파일 예시



#### ESLint (JavaScript/TypeScript)



```json

{

  "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended"],

  "rules": {

    "indent": ["error", 2],

    "quotes": ["error", "single"],

    "semi": ["error", "always"]

  }

}

```



#### .editorconfig (공통)



```ini

root = true



\[\*]

indent\_style = space

indent\_size = 4

end\_of\_line = lf

charset = utf-8

trim\_trailing\_whitespace = true

insert\_final\_newline = true



\[\*.{js,ts,jsx,tsx}]

indent\_size = 2



\[\*.md]

trim\_trailing\_whitespace = false

```



---



## 📖 언어별 빠른 참조



### 명명 규칙 비교표



| 대상 | C# | Python | Java | JavaScript/TypeScript |
|------|-----|--------|------|----------------------|
| 클래스 | PascalCase | PascalCase | PascalCase | PascalCase |
| 함수/메서드 | PascalCase | snake\_case | camelCase | camelCase |
| 변수 | camelCase | snake\_case | camelCase | camelCase |
| 상수 | PascalCase | UPPER\_SNAKE | UPPER\_SNAKE | UPPER\_SNAKE |
| private 필드 | \_camelCase | \_snake\_case | camelCase | \_camelCase / #private |
| 인터페이스 | IName | - | Name | Name |



### 들여쓰기 비교



| 언어 | 권장 들여쓰기 |
|------|--------------|
| C# | 4 spaces |
| Python | 4 spaces |
| Java | 4 spaces (또는 2) |
| JavaScript | 2 spaces |
| TypeScript | 2 spaces |



---



## 📝 변경 이력



| 날짜 | 내용 |
|------|------|
| 2025-12-17 | 초기 문서 생성 (C#, Python, Java, JavaScript, TypeScript) |



---



## 📚 외부 참고 자료



- **C#**: [Microsoft C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- **Python**: [PEP 8 Style Guide](https://peps.python.org/pep-0008/)
- **Java**: [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
- **JavaScript**: [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- **TypeScript**: [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/)



---



*마지막 업데이트: 2025년 12월*



