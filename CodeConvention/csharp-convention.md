# C# 코드 컨벤션 (Code Convention)

이 문서는 C# 프로젝트 전반에서 일관된 코드 스타일을 유지하기 위한 가이드라인을 제공합니다.  
Microsoft의 공식 C# 코딩 규칙을 기반으로 작성되었습니다.

---

## 📋 목차

1. [명명 규칙 (Naming Conventions)](#1-명명-규칙-naming-conventions)
2. [들여쓰기 및 공백 (Indentation & Spacing)](#2-들여쓰기-및-공백-indentation--spacing)
3. [중괄호 스타일 (Brace Style)](#3-중괄호-스타일-brace-style)
4. [주석 규칙 (Comments)](#4-주석-규칙-comments)
5. [클래스 및 인터페이스 (Classes & Interfaces)](#5-클래스-및-인터페이스-classes--interfaces)
6. [메서드 (Methods)](#6-메서드-methods)
7. [변수 및 상수 (Variables & Constants)](#7-변수-및-상수-variables--constants)
8. [LINQ 및 람다 표현식 (LINQ & Lambda)](#8-linq-및-람다-표현식-linq--lambda)
9. [예외 처리 (Exception Handling)](#9-예외-처리-exception-handling)
10. [비동기 프로그래밍 (Async Programming)](#10-비동기-프로그래밍-async-programming)

---

## 1. 명명 규칙 (Naming Conventions)

### 1.1 PascalCase 사용 대상

- **클래스명 (Class)**: `CustomerService`, `OrderManager`
- **메서드명 (Method)**: `GetCustomerById`, `ProcessOrder`
- **프로퍼티 (Property)**: `FirstName`, `TotalAmount`
- **public 필드**: `MaxRetryCount`
- **상수 (Constants)**: `DefaultTimeout`, `MaxBufferSize`
- **열거형 (Enum)**: `OrderStatus`, `PaymentType`
- **이벤트 (Event)**: `OnDataReceived`, `PropertyChanged`

### 1.2 camelCase 사용 대상

- **지역 변수**: `customerName`, `orderTotal`
- **메서드 매개변수**: `userId`, `orderDate`
- **private/protected 필드 (접두사 `_` 사용)**: `_customerRepository`, `_logger`

### 1.3 예시

```csharp
// 클래스명: PascalCase
public class CustomerService
{
    // private 필드: _camelCase
    private readonly ILogger _logger;
    private int _retryCount;

    // 상수: PascalCase
    public const int MaxRetryAttempts = 3;

    // 프로퍼티: PascalCase
    public string CustomerName { get; set; }

    // 메서드: PascalCase, 매개변수: camelCase
    public Customer GetCustomerById(int customerId)
    {
        // 지역 변수: camelCase
        var customerData = FetchFromDatabase(customerId);
        return customerData;
    }
}
```

### 1.4 인터페이스 명명

- **접두사 `I` 사용**: `ICustomerRepository`, `IOrderService`

```csharp
public interface ICustomerRepository
{
    Customer GetById(int id);
    void Save(Customer customer);
}
```

---

## 2. 들여쓰기 및 공백 (Indentation & Spacing)

### 2.1 기본 규칙

- **들여쓰기**: 4 spaces (탭 대신 공백 사용)
- **줄 길이**: 최대 120자 권장
- **파일 끝**: 빈 줄 하나로 종료

### 2.2 공백 규칙

```csharp
// ✅ Good: 연산자 주변에 공백
int result = a + b * c;

// ❌ Bad: 공백 없음
int result=a+b*c;

// ✅ Good: 콤마 후 공백
void Method(int a, int b, string c)

// ✅ Good: 제어문 키워드 후 공백
if (condition)
for (int i = 0; i < 10; i++)
while (running)

// ❌ Bad: 괄호 안쪽에 불필요한 공백
if ( condition )
Method( param1, param2 )
```

---

## 3. 중괄호 스타일 (Brace Style)

### 3.1 Allman 스타일 (권장)

C#에서는 **새 줄에 중괄호**를 여는 Allman 스타일을 권장합니다.

```csharp
// ✅ Good: Allman Style
public class CustomerService
{
    public void ProcessOrder(Order order)
    {
        if (order.IsValid)
        {
            // 처리 로직
        }
        else
        {
            // 에러 처리
        }
    }
}
```

### 3.2 단일 문장도 중괄호 사용

```csharp
// ✅ Good: 중괄호 사용
if (condition)
{
    DoSomething();
}

// ❌ Bad: 중괄호 생략
if (condition)
    DoSomething();
```

---

## 4. 주석 규칙 (Comments)

### 4.1 XML 문서 주석

public API에는 반드시 XML 문서 주석을 작성합니다.

```csharp
/// <summary>
/// 고객 정보를 ID로 조회합니다.
/// </summary>
/// <param name="customerId">조회할 고객의 고유 ID</param>
/// <returns>고객 정보 객체. 존재하지 않으면 null 반환</returns>
/// <exception cref="ArgumentException">customerId가 0 이하인 경우</exception>
public Customer GetCustomerById(int customerId)
{
    if (customerId <= 0)
    {
        throw new ArgumentException("Customer ID must be positive", nameof(customerId));
    }
    
    return _repository.FindById(customerId);
}
```

### 4.2 인라인 주석

```csharp
// 단일 행 주석은 코드 위에 작성
var timeout = 30000;  // 밀리초 단위 (30초)

/*
 * 여러 줄 주석은
 * 이 형식을 사용합니다.
 */
```

### 4.3 TODO/FIXME 주석

```csharp
// TODO: 성능 최적화 필요 - 대량 데이터 처리 시 병목 발생
// FIXME: 엣지 케이스 처리 누락 - null 입력 시 예외 발생
// HACK: 임시 해결책 - API v2에서 수정 예정
```

---

## 5. 클래스 및 인터페이스 (Classes & Interfaces)

### 5.1 클래스 구조 순서

```csharp
public class CustomerService : ICustomerService
{
    // 1. 상수 (Constants)
    private const int MaxRetryCount = 3;

    // 2. 정적 필드 (Static fields)
    private static readonly object _lock = new object();

    // 3. 인스턴스 필드 (Instance fields)
    private readonly ILogger _logger;
    private readonly ICustomerRepository _repository;

    // 4. 생성자 (Constructors)
    public CustomerService(ILogger logger, ICustomerRepository repository)
    {
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
        _repository = repository ?? throw new ArgumentNullException(nameof(repository));
    }

    // 5. 프로퍼티 (Properties)
    public int TotalCustomers { get; private set; }

    // 6. public 메서드 (Public methods)
    public Customer GetById(int id) { /* ... */ }

    // 7. protected 메서드 (Protected methods)
    protected virtual void OnCustomerLoaded() { /* ... */ }

    // 8. private 메서드 (Private methods)
    private void ValidateInput(int id) { /* ... */ }
}
```

### 5.2 한 파일에 하나의 클래스

- 각 클래스는 별도의 파일에 작성
- 파일명은 클래스명과 동일: `CustomerService.cs`

---

## 6. 메서드 (Methods)

### 6.1 메서드 길이

- **권장 최대 줄 수**: 30줄
- 길어지면 작은 메서드로 분리

### 6.2 매개변수 규칙

```csharp
// ✅ Good: 매개변수 3-4개 이하
public void CreateOrder(int customerId, string productCode, int quantity)

// 매개변수가 많으면 객체로 캡슐화
public void CreateOrder(OrderRequest request)

// 긴 매개변수 목록은 줄바꿈
public void ProcessComplexOperation(
    int operationId,
    string operationType,
    DateTime scheduledTime,
    IList<int> targetIds)
{
    // ...
}
```

### 6.3 반환값 규칙

```csharp
// 빈 컬렉션 반환 (null 대신)
public IList<Customer> GetAllCustomers()
{
    var customers = _repository.GetAll();
    return customers ?? new List<Customer>();
}

// nullable 반환 시 명시
public Customer? FindCustomerByEmail(string email)
{
    return _repository.FindByEmail(email);
}
```

---

## 7. 변수 및 상수 (Variables & Constants)

### 7.1 var 키워드 사용

```csharp
// ✅ Good: 타입이 명확할 때 var 사용
var customers = new List<Customer>();
var name = "John Doe";
var count = GetCustomerCount();  // 반환 타입이 명확한 경우

// ✅ Good: 타입이 불명확할 때 명시적 타입
IEnumerable<Customer> customers = GetCustomers();
```

### 7.2 상수 정의

```csharp
// 컴파일 타임 상수
public const int MaxRetries = 3;
public const string DefaultConnectionString = "Server=localhost";

// 런타임 상수 (readonly)
public static readonly TimeSpan DefaultTimeout = TimeSpan.FromSeconds(30);
private readonly DateTime _createdAt = DateTime.UtcNow;
```

### 7.3 매직 넘버 제거

```csharp
// ❌ Bad: 매직 넘버
if (retryCount > 3)
Thread.Sleep(30000);

// ✅ Good: 상수 사용
private const int MaxRetryCount = 3;
private const int RetryDelayMilliseconds = 30000;

if (retryCount > MaxRetryCount)
Thread.Sleep(RetryDelayMilliseconds);
```

---

## 8. LINQ 및 람다 표현식 (LINQ & Lambda)

### 8.1 LINQ 쿼리 포맷

```csharp
// 메서드 구문 (짧은 쿼리)
var activeCustomers = customers.Where(c => c.IsActive).ToList();

// 메서드 구문 (긴 쿼리 - 줄바꿈)
var result = customers
    .Where(c => c.IsActive)
    .OrderBy(c => c.LastName)
    .ThenBy(c => c.FirstName)
    .Select(c => new CustomerDto
    {
        Id = c.Id,
        FullName = $"{c.FirstName} {c.LastName}"
    })
    .ToList();

// 쿼리 구문 (복잡한 조인)
var query = from c in customers
            join o in orders on c.Id equals o.CustomerId
            where c.IsActive
            orderby c.LastName
            select new { Customer = c, Order = o };
```

### 8.2 람다 표현식

```csharp
// 단순 람다
Func<int, int> square = x => x * x;

// 복잡한 람다는 메서드로 분리
customers.Where(IsEligibleForDiscount);

private bool IsEligibleForDiscount(Customer customer)
{
    return customer.IsActive 
        && customer.TotalPurchases > 1000 
        && customer.MembershipYears >= 2;
}
```

---

## 9. 예외 처리 (Exception Handling)

### 9.1 기본 규칙

```csharp
try
{
    // 최소한의 코드만 try 블록에
    var result = ProcessData(data);
    return result;
}
catch (SpecificException ex)
{
    // 구체적인 예외부터 처리
    _logger.LogError(ex, "Data processing failed for {DataId}", data.Id);
    throw;  // 재throw 시 스택 트레이스 보존
}
catch (Exception ex)
{
    // 일반 예외는 마지막에
    _logger.LogError(ex, "Unexpected error");
    throw new ApplicationException("An error occurred", ex);
}
finally
{
    // 리소스 정리
    CleanupResources();
}
```

### 9.2 예외 생성

```csharp
// Guard 절 패턴
public void ProcessOrder(Order order)
{
    if (order == null)
    {
        throw new ArgumentNullException(nameof(order));
    }

    if (order.Items.Count == 0)
    {
        throw new ArgumentException("Order must have at least one item", nameof(order));
    }

    // 비즈니스 로직
}

// 사용자 정의 예외
public class OrderProcessingException : Exception
{
    public int OrderId { get; }

    public OrderProcessingException(int orderId, string message)
        : base(message)
    {
        OrderId = orderId;
    }

    public OrderProcessingException(int orderId, string message, Exception inner)
        : base(message, inner)
    {
        OrderId = orderId;
    }
}
```

---

## 10. 비동기 프로그래밍 (Async Programming)

### 10.1 async/await 규칙

```csharp
// ✅ Good: Async 접미사 사용
public async Task<Customer> GetCustomerByIdAsync(int id)
{
    return await _repository.FindByIdAsync(id);
}

// ✅ Good: ConfigureAwait 사용 (라이브러리 코드)
public async Task<string> FetchDataAsync()
{
    var response = await _httpClient.GetAsync(url).ConfigureAwait(false);
    return await response.Content.ReadAsStringAsync().ConfigureAwait(false);
}

// ❌ Bad: async void (이벤트 핸들러 제외)
public async void ProcessData() { }  // 사용 금지

// ✅ Good: async Task 반환
public async Task ProcessDataAsync() { }
```

### 10.2 취소 토큰 (CancellationToken)

```csharp
public async Task<IList<Customer>> GetAllCustomersAsync(
    CancellationToken cancellationToken = default)
{
    cancellationToken.ThrowIfCancellationRequested();
    
    return await _repository
        .GetAllAsync(cancellationToken)
        .ConfigureAwait(false);
}
```

---

## 📚 참고 자료

- [Microsoft C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [.NET Framework Design Guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/)
- [C# at Google Style Guide](https://google.github.io/styleguide/csharp-style.html)

---

*마지막 업데이트: 2025년 12월*
