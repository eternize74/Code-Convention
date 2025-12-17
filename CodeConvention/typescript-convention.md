# TypeScript 코드 컨벤션 (Code Convention)

TypeScript 공식 가이드라인 및 커뮤니티 권장사항 기반 코딩 규칙입니다.

---

## 1. 명명 규칙

| 대상 | 스타일 | 예시 |
|------|--------|------|
| 변수/함수 | camelCase | `userName`, `getUserById()` |
| 클래스 | PascalCase | `CustomerService` |
| 인터페이스 | PascalCase | `UserData` (I 접두사 금지) |
| 타입 별칭 | PascalCase | `UserId`, `RequestHandler` |
| 열거형 | PascalCase | `OrderStatus` |
| 제네릭 | 단일 대문자 또는 설명적 | `T`, `TKey`, `TValue` |
| 상수 | UPPER_SNAKE_CASE | `MAX_SIZE` |

```typescript
// 상수
const MAX_RETRY_COUNT = 3;

// 인터페이스 (I 접두사 사용 안 함)
interface UserData {
  id: number;
  name: string;
  email: string;
}

// 타입 별칭
type UserId = number;
type RequestHandler = (req: Request) => Response;

// 클래스
class CustomerService {
  private repository: CustomerRepository;
  
  constructor(repository: CustomerRepository) {
    this.repository = repository;
  }
  
  getCustomerById(customerId: number): Customer | null {
    return this.repository.findById(customerId);
  }
}
```

---

## 2. 타입 정의

### 2.1 기본 타입

```typescript
// 기본 타입
const name: string = 'John';
const age: number = 30;
const isActive: boolean = true;

// 배열
const numbers: number[] = [1, 2, 3];
const names: Array<string> = ['Alice', 'Bob'];  // 제네릭 형태

// 튜플
const point: [number, number] = [10, 20];
const nameAge: [string, number] = ['John', 30];

// 객체
const user: { name: string; age: number } = {
  name: 'John',
  age: 30
};
```

### 2.2 인터페이스 vs 타입

```typescript
// interface: 객체 구조, 확장 가능
interface User {
  id: number;
  name: string;
}

interface Admin extends User {
  role: string;
  permissions: string[];
}

// type: 유니온, 인터섹션, 복잡한 타입
type UserId = number | string;
type UserWithRole = User & { role: string };
type Status = 'pending' | 'active' | 'inactive';

// 권장: 객체 구조는 interface, 나머지는 type
interface UserData {
  id: number;
  name: string;
}

type UserStatus = 'active' | 'inactive';
```

### 2.3 제네릭

```typescript
// 제네릭 함수
function first<T>(items: T[]): T | undefined {
  return items[0];
}

// 제네릭 인터페이스
interface Repository<T> {
  findById(id: number): T | null;
  save(entity: T): T;
  delete(id: number): boolean;
}

// 제한된 제네릭
interface HasId {
  id: number;
}

function updateEntity<T extends HasId>(entity: T): T {
  console.log(`Updating entity ${entity.id}`);
  return entity;
}

// 기본값이 있는 제네릭
interface ApiResponse<T = unknown> {
  data: T;
  status: number;
  message: string;
}
```

---

## 3. 함수 타입

```typescript
// 함수 선언
function add(a: number, b: number): number {
  return a + b;
}

// 화살표 함수
const multiply = (a: number, b: number): number => a * b;

// 선택적 매개변수
function greet(name: string, greeting?: string): string {
  return `${greeting ?? 'Hello'}, ${name}!`;
}

// 기본값 매개변수
function createUser(name: string, age: number = 0): User {
  return { name, age };
}

// 나머지 매개변수
function sum(...numbers: number[]): number {
  return numbers.reduce((a, b) => a + b, 0);
}

// 함수 타입 별칭
type Comparator<T> = (a: T, b: T) => number;

// 오버로드
function parse(input: string): object;
function parse(input: object): string;
function parse(input: string | object): string | object {
  if (typeof input === 'string') {
    return JSON.parse(input);
  }
  return JSON.stringify(input);
}
```

---

## 4. 클래스

```typescript
class CustomerService {
  // 접근 제어자
  private readonly repository: CustomerRepository;
  protected logger: Logger;
  public pageSize: number = 10;
  
  // static
  static readonly DEFAULT_PAGE_SIZE = 10;
  
  constructor(repository: CustomerRepository, logger: Logger) {
    this.repository = repository;
    this.logger = logger;
  }
  
  // 메서드
  async getCustomerById(id: number): Promise<Customer | null> {
    try {
      return await this.repository.findById(id);
    } catch (error) {
      this.logger.error('Failed to get customer', error);
      return null;
    }
  }
  
  // getter/setter
  get isEmpty(): boolean {
    return this.pageSize === 0;
  }
  
  set size(value: number) {
    if (value > 0) {
      this.pageSize = value;
    }
  }
}

// 추상 클래스
abstract class BaseService<T> {
  abstract findById(id: number): Promise<T | null>;
  abstract save(entity: T): Promise<T>;
  
  protected log(message: string): void {
    console.log(`[${this.constructor.name}] ${message}`);
  }
}
```

---

## 5. 유틸리티 타입

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

// Partial: 모든 속성 선택적
type PartialUser = Partial<User>;

// Required: 모든 속성 필수
type RequiredUser = Required<PartialUser>;

// Pick: 특정 속성만 선택
type UserBasic = Pick<User, 'id' | 'name'>;

// Omit: 특정 속성 제외
type UserWithoutEmail = Omit<User, 'email'>;

// Record: 키-값 매핑
type UserMap = Record<number, User>;

// Readonly: 읽기 전용
type ReadonlyUser = Readonly<User>;

// ReturnType: 함수 반환 타입
type FetchResult = ReturnType<typeof fetchData>;

// Parameters: 함수 매개변수 타입
type FetchParams = Parameters<typeof fetchData>;

// NonNullable: null/undefined 제외
type ValidUser = NonNullable<User | null>;
```

---

## 6. 타입 가드

```typescript
// typeof 가드
function process(value: string | number): string {
  if (typeof value === 'string') {
    return value.toUpperCase();
  }
  return value.toString();
}

// instanceof 가드
function handle(error: Error | TypeError): void {
  if (error instanceof TypeError) {
    console.log('Type error:', error.message);
  } else {
    console.log('Error:', error.message);
  }
}

// in 연산자 가드
interface Dog { bark(): void; }
interface Cat { meow(): void; }

function speak(animal: Dog | Cat): void {
  if ('bark' in animal) {
    animal.bark();
  } else {
    animal.meow();
  }
}

// 사용자 정의 타입 가드
function isUser(value: unknown): value is User {
  return (
    typeof value === 'object' &&
    value !== null &&
    'id' in value &&
    'name' in value
  );
}

// 사용
if (isUser(data)) {
  console.log(data.name);  // User 타입으로 추론
}
```

---

## 7. 비동기 타입

```typescript
// Promise 타입
async function fetchUser(id: number): Promise<User> {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

// 반환 타입 명시
async function fetchUsers(): Promise<User[]> {
  const response = await fetch('/api/users');
  const data: User[] = await response.json();
  return data;
}

// 에러 처리
async function safeCall<T>(
  fn: () => Promise<T>
): Promise<[T, null] | [null, Error]> {
  try {
    const result = await fn();
    return [result, null];
  } catch (error) {
    return [null, error as Error];
  }
}
```

---

## 8. 모듈

```typescript
// named export
export const MAX_SIZE = 100;
export function helper(): void { }
export class Service { }
export interface Config { }
export type Handler = () => void;

// default export
export default class MainService { }

// named import
import { MAX_SIZE, helper, type Config } from './utils';

// 타입 전용 import
import type { User, Order } from './types';

// 혼합 import
import MainService, { helper } from './module';

// 전체 import
import * as utils from './utils';
```

---

## 9. 엄격 모드 설정 (tsconfig.json)

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

---

## 10. 주석 및 JSDoc

```typescript
/**
 * 고객 서비스를 제공하는 클래스
 * @template T - 고객 엔티티 타입
 */
class CustomerService<T extends User> {
  /**
   * ID로 고객을 조회합니다
   * @param id - 고객 고유 ID
   * @returns 고객 객체 또는 null
   * @throws {ValidationError} ID가 유효하지 않은 경우
   * @example
   * ```typescript
   * const customer = await service.getById(123);
   * ```
   */
  async getById(id: number): Promise<T | null> {
    if (id <= 0) {
      throw new ValidationError('Invalid ID');
    }
    return this.repository.findById(id);
  }
}

// 타입 주석
/** 사용자 ID 타입 */
type UserId = number;

/** 주문 상태 */
type OrderStatus = 'pending' | 'confirmed' | 'shipped';
```

---

## 📚 참고 자료

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [Google TypeScript Style Guide](https://google.github.io/styleguide/tsguide.html)

---

*마지막 업데이트: 2025년 12월*
