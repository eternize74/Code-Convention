# JavaScript 코드 컨벤션 (Code Convention)

Airbnb JavaScript Style Guide 기반 JavaScript 코딩 규칙입니다.

---

## 1. 명명 규칙

| 대상 | 스타일 | 예시 |
|------|--------|------|
| 변수/함수 | camelCase | `userName`, `getUserById()` |
| 클래스/생성자 | PascalCase | `CustomerService` |
| 상수 | UPPER_SNAKE_CASE | `MAX_SIZE`, `API_URL` |
| private (관례) | _접두사 | `_internalData` |
| 파일명 | kebab-case | `user-service.js` |

```javascript
// 상수
const MAX_RETRY_COUNT = 3;
const API_BASE_URL = 'https://api.example.com';

// 클래스
class CustomerService {
    constructor(repository) {
        this._repository = repository;
    }
    
    getCustomerById(customerId) {
        return this._repository.findById(customerId);
    }
}

// 함수
function calculateTotalPrice(items) {
    return items.reduce((sum, item) => sum + item.price, 0);
}
```

---

## 2. 변수 선언

```javascript
// ✅ const 우선 사용 (재할당 없음)
const userId = 123;
const config = { timeout: 5000 };

// ✅ 재할당 필요시 let 사용
let count = 0;
count += 1;

// ❌ var 사용 금지
var oldStyle = 'avoid';

// 변수 선언은 사용 직전에
function process() {
    const data = fetchData();
    
    // ... 다른 코드 ...
    
    const result = transform(data);  // 사용 직전 선언
    return result;
}
```

---

## 3. 들여쓰기 및 공백

- **들여쓰기**: 2 spaces
- **줄 길이**: 최대 80-100자
- **세미콜론**: 항상 사용

```javascript
// ✅ Good
function processOrder(order) {
  if (order.isValid) {
    return calculateTotal(order);
  }
  return null;
}

// 연산자 주변 공백
const result = a + b * c;
const isValid = value > 0 && value < 100;

// 객체/배열
const user = { name: 'John', age: 30 };
const items = [1, 2, 3];

// 함수 호출
doSomething(arg1, arg2);
```

---

## 4. 함수

### 4.1 화살표 함수 (권장)

```javascript
// 간단한 표현식
const double = x => x * 2;
const add = (a, b) => a + b;

// 블록 필요시
const process = (data) => {
  const validated = validate(data);
  return transform(validated);
};

// 배열 메서드와 함께
const activeUsers = users.filter(user => user.isActive);
const names = users.map(user => user.name);
```

### 4.2 함수 선언

```javascript
// 함수 선언 (호이스팅 필요시)
function fetchUserData(userId) {
  return api.get(`/users/${userId}`);
}

// 함수 표현식 (권장)
const fetchUserData = function(userId) {
  return api.get(`/users/${userId}`);
};

// 기본값 매개변수
function greet(name = 'Guest', greeting = 'Hello') {
  return `${greeting}, ${name}!`;
}

// 나머지 매개변수
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

---

## 5. 객체와 배열

### 5.1 객체

```javascript
// 속성 축약
const name = 'John';
const age = 30;
const user = { name, age };  // { name: name, age: age }

// 메서드 축약
const service = {
  getData() {
    return this.data;
  },
  
  async fetchData() {
    return await api.get('/data');
  }
};

// 계산된 속성명
const key = 'dynamicKey';
const obj = {
  [key]: 'value',
  [`${key}Count`]: 10
};
```

### 5.2 구조 분해

```javascript
// 객체 구조 분해
const { name, age, email = 'N/A' } = user;

// 중첩 구조 분해
const { address: { city, country } } = user;

// 이름 변경
const { name: userName } = user;

// 배열 구조 분해
const [first, second, ...rest] = items;
const [, , third] = items;  // 건너뛰기

// 함수 매개변수
function printUser({ name, age }) {
  console.log(`${name} is ${age}`);
}
```

### 5.3 스프레드 연산자

```javascript
// 배열 복사/병합
const copy = [...original];
const merged = [...arr1, ...arr2];

// 객체 복사/병합
const userCopy = { ...user };
const updated = { ...user, name: 'New Name' };

// 함수 인자 전달
const numbers = [1, 2, 3];
Math.max(...numbers);
```

---

## 6. 클래스

```javascript
class CustomerService {
  // private 필드 (ES2022)
  #repository;
  #logger;
  
  // static 필드
  static DEFAULT_PAGE_SIZE = 10;
  
  constructor(repository, logger) {
    this.#repository = repository;
    this.#logger = logger;
  }
  
  // getter/setter
  get pageSize() {
    return this._pageSize ?? CustomerService.DEFAULT_PAGE_SIZE;
  }
  
  set pageSize(value) {
    if (value > 0) {
      this._pageSize = value;
    }
  }
  
  // public 메서드
  async getCustomerById(id) {
    try {
      return await this.#repository.findById(id);
    } catch (error) {
      this.#logger.error('Failed to get customer', error);
      throw error;
    }
  }
  
  // static 메서드
  static createDefault() {
    return new CustomerService(new Repository(), console);
  }
}
```

---

## 7. 비동기 처리

### 7.1 async/await (권장)

```javascript
// ✅ async/await 사용
async function fetchUserData(userId) {
  try {
    const response = await api.get(`/users/${userId}`);
    const userData = await response.json();
    return userData;
  } catch (error) {
    console.error('Fetch failed:', error);
    throw error;
  }
}

// 병렬 실행
async function fetchAll() {
  const [users, orders] = await Promise.all([
    fetchUsers(),
    fetchOrders()
  ]);
  return { users, orders };
}
```

### 7.2 Promise

```javascript
// Promise 생성
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Promise 체이닝 (async/await이 더 권장됨)
fetchData()
  .then(data => process(data))
  .then(result => save(result))
  .catch(error => console.error(error))
  .finally(() => cleanup());
```

---

## 8. 모듈

```javascript
// named export
export const MAX_SIZE = 100;
export function helper() { }
export class Service { }

// default export
export default class MainService { }

// named import
import { MAX_SIZE, helper } from './utils.js';

// default import
import MainService from './main-service.js';

// 혼합 import
import MainService, { helper } from './module.js';

// 별칭
import { veryLongFunctionName as fn } from './utils.js';

// 전체 import
import * as utils from './utils.js';
```

---

## 9. 조건문 및 비교

```javascript
// ✅ 엄격한 동등 비교 (===, !==)
if (value === null) { }
if (value !== undefined) { }

// ❌ 느슨한 비교 금지
if (value == null) { }

// Truthy/Falsy 활용
if (items.length) { }    // 비어있지 않으면
if (!items.length) { }   // 비어있으면

// 삼항 연산자 (간단한 경우만)
const status = isActive ? 'Active' : 'Inactive';

// Nullish coalescing (??)
const name = user.name ?? 'Guest';

// Optional chaining (?.)
const city = user?.address?.city;
const result = obj?.method?.();
```

---

## 10. 주석

```javascript
/**
 * 주문 총액을 계산합니다.
 * @param {Array<Object>} items - 주문 항목 배열
 * @param {number} discountRate - 할인율 (0-1)
 * @returns {number} 할인 적용된 총액
 * @throws {Error} items가 비어있는 경우
 */
function calculateTotal(items, discountRate = 0) {
  if (!items.length) {
    throw new Error('Items cannot be empty');
  }
  
  const subtotal = items.reduce((sum, item) => sum + item.price, 0);
  return subtotal * (1 - discountRate);
}

// 인라인 주석
const timeout = 30000; // 밀리초 단위

// TODO/FIXME
// TODO: 캐싱 구현 필요
// FIXME: 엣지 케이스 처리 누락
```

---

## 📚 참고 자료

- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)

---

*마지막 업데이트: 2025년 12월*
