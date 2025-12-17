# Python 코드 컨벤션 (Code Convention)

PEP 8 스타일 가이드 기반 Python 코딩 규칙입니다.

---

## 1. 명명 규칙

| 대상 | 스타일 | 예시 |
|------|--------|------|
| 변수/함수/메서드 | snake_case | `user_name`, `get_data()` |
| 클래스 | PascalCase | `CustomerService` |
| 상수 | UPPER_SNAKE_CASE | `MAX_SIZE` |
| Private | _접두사 | `_internal_var` |

```python
# 상수
MAX_CONNECTIONS = 100

# 클래스
class CustomerRepository:
    def __init__(self, connection_string: str):
        self._connection = connection_string
    
    def get_customer_by_id(self, customer_id: int) -> dict:
        query_result = self._execute_query(customer_id)
        return query_result
```

---

## 2. 들여쓰기 및 공백

- **들여쓰기**: 4 spaces (탭 금지)
- **줄 길이**: 최대 79자 (또는 120자)

```python
# ✅ Good
result = a + b * c
my_list = [1, 2, 3]
def function(arg1, arg2):
    pass

# ❌ Bad
result=a+b*c
function( arg1, arg2 )
```

---

## 3. Import 순서

```python
# 1. 표준 라이브러리
import os
from datetime import datetime

# 2. 서드파티
import numpy as np
from flask import Flask

# 3. 로컬
from myapp.models import User
```

---

## 4. Docstring (Google 스타일)

```python
def calculate_discount(price: float, rate: float) -> float:
    """할인 가격을 계산합니다.
    
    Args:
        price: 원래 가격
        rate: 할인율 (0.0 ~ 1.0)
    
    Returns:
        할인 적용된 가격
    
    Raises:
        ValueError: rate가 범위를 벗어난 경우
    """
    return price * (1 - rate)
```

---

## 5. 타입 힌트

```python
from typing import Optional, List

def get_users(active: bool = True) -> List[dict]:
    pass

def find_user(user_id: int) -> Optional[dict]:
    pass

# Python 3.10+
def process(data: int | str) -> None:
    pass
```

---

## 6. 예외 처리

```python
# ✅ Good: 구체적 예외 처리
try:
    value = int(user_input)
except ValueError:
    print("Invalid number")
except Exception as e:
    logger.error(f"Error: {e}")
    raise

# 컨텍스트 매니저 사용
with open("file.txt") as f:
    data = f.read()
```

---

## 7. 조건문 및 비교

```python
# None 비교
if value is None:
    pass

# Boolean 직접 사용
if is_active:
    pass

# 빈 시퀀스 검사
if items:  # 비어있지 않으면
    pass
```

---

## 8. 문자열 포맷팅

```python
name = "Alice"
age = 30

# f-string 사용 (권장)
message = f"Hello, {name}! Age: {age}"
```

---

## 📚 참고 자료

- [PEP 8](https://peps.python.org/pep-0008/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)

---

*마지막 업데이트: 2025년 12월*
